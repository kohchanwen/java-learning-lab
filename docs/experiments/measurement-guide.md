# Measuring experiments

Use the same process for every comparative experiment. For file processing,
compare full-file loading with incremental processing using the same input,
output contract, and JVM settings.

Status: this guide describes the planned measurement implementation. The runner,
heap sampler, input generator, and example Java classes below do not exist yet.

## Measurement tools and their limits

| Measurement | Tool | Meaning and limitation |
| --- | --- | --- |
| Whole-process elapsed and CPU time | `/usr/bin/time -v` | Includes JVM startup, processing, monitoring, and shutdown |
| Peak resident process memory | `/usr/bin/time -v` | Maximum resident set size in KB on Linux; includes more than Java heap |
| Processing elapsed time | `System.nanoTime()` around the operation | Includes reading, parsing, and aggregation; excludes dataset generation and final report writing |
| Heap usage over time | Periodic `MemoryMXBean.getHeapMemoryUsage()` readings | Reports used, committed, and maximum heap at sampling points |
| Maximum sampled heap usage | Highest recorded `used` value | An observed maximum; can miss short spikes between samples |
| Heap usage around garbage collections | JFR `jdk.GCHeapSummary` events | Before/after GC observations; not a continuous or exact peak measurement |
| Records, totals, queue depth, cache hits | Counters in the experiment | Verify correctness and explain application behavior |

The Java heap contains live objects and garbage not yet collected. Committed
heap is memory available for the JVM to use; it is not the same as used heap.
`-Xmx` limits Java heap, not total process memory. Do not sum the peaks of separate
heap pools and label that the whole-heap peak: those peaks may occur at different times.

## Measurement process

1. **Specify the contract.** Define record format, encoding, malformed-input
   handling, and expected count and total. Every approach must do equivalent work.
2. **Prove correctness on small inputs.** Test empty input, boundary values,
   malformed records, missing final newline, and large individual records.
3. **Generate input before measuring.** Record the generator command, seed,
   record count, file bytes, largest record, and expected output. Generate
   incrementally so generation does not require a complete in-memory dataset.
4. **Fix the environment.** Record Java version, OS/WSL details, JVM options,
   input location, and buffer/batch/worker settings. Use the same filesystem and
   input file for both approaches; avoid comparing a WSL Linux path with a
   Windows-mounted path. Keep unrelated heavy workloads to a minimum.
5. **Start each measured run in a fresh JVM.** Use the same `-Xms`, `-Xmx`,
   sampler configuration, and implementation version. Time the Java process
   directly, rather than timing Maven compilation or the JUnit test runner.
6. **Sample heap while the operation runs.** Take an initial sample, periodic
   samples, and a final sample. Keep actual timestamps, not assumed intervals.
7. **Repeat both approaches.** Begin with five measured runs per approach and
   input size. Alternate which approach runs first. Report median and range,
   retaining individual results. State the warm-up and filesystem-cache policy;
   repeated file reads can benefit from caching. A fresh JVM starts its own JIT
   warm-up each time, even if an earlier process ran the same code.
8. **Check every successful run.** Match the expected count and total. An
   incomplete or incorrect run is not a faster implementation.
9. **Investigate with JFR.** Make additional diagnostic runs with recording
   enabled for both approaches. Keep them separately labeled because recording
   adds overhead. If comparing profiled timings, use identical recording settings.
10. **Report limits and failures.** Record nonzero exit status, timeout, or
    out-of-memory failure. An OutOfMemoryError is not a malformed-record error.
    Abrupt termination can leave sampling output incomplete; label it as partial.

Do not print each record, retain every processed result in the incremental
implementation, or call `System.gc()` to manufacture clean measurement points.
Let garbage collection happen naturally. Size the inputs to local resources;
roughly 1 MB and 100 MB are reasonable initial cases, followed by a larger input.

## Implementation steps

### 1. Define comparable processors

Under `src/main/java/learning/experiments/fileprocessing/`, plan these classes:

- `FullLoadProcessor`: read the complete file, then parse and aggregate.
- `StreamingProcessor`: read one record at a time, update count and total, and
  release the record instead of collecting all records into a list.
- `ProcessingResult`: return record count and total so both implementations can
  be checked against the same expected output.
- `FileProcessingExperiment`: parse arguments, choose the processor, control
  measurement boundaries, and write the run summary.

Use the same parsing rules in both processors. For money, define integer minor
units or decimal arithmetic and rounding consistently. Use a buffered reader and
try-with-resources. Remember that line-by-line processing still retains one
whole line, so maximum record length affects memory use.

### 2. Build a reusable periodic heap sampler

Plan a `HeapSampler` helper under
`src/main/java/learning/experiments/measurement/`. It should accept an interval
and output path, and implement `AutoCloseable` for predictable cleanup.

Read each sample through the standard Java management API:

```java
import java.lang.management.ManagementFactory;
import java.lang.management.MemoryMXBean;
import java.lang.management.MemoryUsage;

MemoryMXBean memory = ManagementFactory.getMemoryMXBean();

// Repeat this read on the sampler's background thread.
MemoryUsage usage = memory.getHeapMemoryUsage();
long usedBytes = usage.getUsed();
long committedBytes = usage.getCommitted();
long maxBytes = usage.getMax();
```

This snippet only shows how to obtain one sample; implement the lifecycle and
output below as part of the experiment infrastructure.

- Start with a **100 ms requested interval**, configurable for later experiments.
  Report it alongside the results. Shorter intervals increase monitoring overhead
  and still cannot guarantee capturing the true maximum.
- Use one background thread, for example a single-thread
  `ScheduledExecutorService` with `scheduleWithFixedDelay`. Fixed delay avoids
  bursts of catch-up reads after a slow sample, but actual spacing also includes
  sampling and writing time. GC pauses and scheduling can delay any sampler.
- Take an initial reading before processing. Timestamp readings relative to a
  common `System.nanoTime()` origin, converting elapsed nanoseconds to milliseconds.
- Write rows incrementally to a buffered CSV writer. Keep only running maximum,
  sample count, and other small summary state; do not accumulate every sample in
  an unbounded list. Do not flush or print to the terminal after every row.
- Keep used, committed, and max values in bytes. Preserve `-1` when the maximum
  is undefined rather than treating it as a measured memory size.
- Keep CSV writes and peak updates serialized. Initial, periodic, and final
  readings must not race with each other or with closing the writer.
- On stop, cancel further sampling and wait for an in-flight read/write to finish,
  then take the final sample and flush/close the writer. Shut down the executor
  in a `finally` block or through try-with-resources, including processing failures.
- Capture background sampling failures and surface them to the runner. Do not
  silently produce a successful-looking report if sampling stopped midway.

CSV format:

```csv
elapsed_ms,phase,heap_used_bytes,heap_committed_bytes,heap_max_bytes
```

Use phases such as `before`, `processing`, and `after` to make the boundaries
clear. Report the largest observed used value as **maximum sampled heap usage**.
A final reading after completion cannot replace samples taken during processing.
A run shorter than the interval may have no periodic samples; record the sample
count and use a larger workload when studying the timeline.

The sampler and its output buffers also occupy heap and consume CPU. This is
whole-JVM used heap, not the exact memory attributable to the processor. Use the
same sampler for both approaches and report monitoring overhead as a limitation.
Do not subtract the initial heap reading and call the result total allocations:
allocation and reclamation happen throughout the run.

### 3. Put timing boundaries in the runner

Implement a command-line contract such as:

```text
FileProcessingExperiment <full-load|streaming> <input-file> <output-prefix> <sample-interval-ms>
```

The planned runner should:

1. Validate arguments and prepare its report paths before timing starts.
2. Initialize the sampler and take the baseline sample.
3. Read `System.nanoTime()` immediately before invoking the selected processor.
4. Run all input reading, parsing, and aggregation inside the timed operation.
5. Capture the finishing time and result when processing returns.
6. Stop/join the sampler and finish its CSV outside that processing timer.
7. Write `<output-prefix>-summary.csv` with approach, interval, sample count,
   processing duration, maximum sampled heap usage, record count, total, and status.
   Write samples to `<output-prefix>-heap.csv`.
8. Return a nonzero exit status on failure. The external process runner must
   also record failures when the JVM cannot write its own summary.

The processing duration still includes interference from the concurrent sampler
and GC pauses. State that fact when comparing runs. Calculate records per second
using the verified record count and the documented timing boundary.

### 4. Launch the process and capture resource usage

The commands below are **templates for after the classes and dataset exist**;
they do not run against the current empty scaffold.

Compile once outside measurement:

```bash
mvn test
mkdir -p results/file-processing
```

Example measured run, with a 100 ms sampling interval:

```bash
/usr/bin/time -v \
  -o results/file-processing/full-load-01-time.txt \
  java -Xms64m -Xmx256m -cp target/classes \
  learning.experiments.fileprocessing.FileProcessingExperiment \
  full-load data/generated/transactions.csv \
  results/file-processing/full-load-01 100
```

Repeat with `streaming` and a distinct output prefix. Use distinct run IDs for
all repetitions so reports are not overwritten. Keep JVM settings identical.
The plain classpath above is suitable for a JDK-only file experiment; a later
JSON experiment will also need its library dependencies on the runtime classpath.

### 5. Add a separate JFR diagnostic run

```bash
java -Xms64m -Xmx256m \
  -XX:StartFlightRecording=filename=results/file-processing/full-load-jfr-01.jfr,settings=profile,dumponexit=true \
  -cp target/classes \
  learning.experiments.fileprocessing.FileProcessingExperiment \
  full-load data/generated/transactions.csv \
  results/file-processing/full-load-jfr-01 100

jfr print --events jdk.GCHeapSummary \
  results/file-processing/full-load-jfr-01.jfr
```

Repeat for streaming using separate filenames. Match before/after events by
`gcId` to see how heap usage changes across a collection. A run with no GC may
have no heap-summary events; absence of events does not mean zero heap usage.
Periodic sampling provides observations between those events, subject to its
own timing gaps. Do not present either measurement as the exact true peak.

### 6. Verify the measurement infrastructure

Before interpreting results, check that:

- Both processors pass the same correctness tests and produce expected totals.
- A deliberately long run produces timestamped samples in increasing order.
- The recorded sampled maximum equals the largest used value in its CSV.
- Initial/final samples exist for a successful short run, even when no periodic
  sample occurs. Tests do not depend on exact scheduling or heap byte counts.
- Processing failure and sampling failure close resources and stop background
  threads, rather than leaving the JVM alive or silently dropping monitoring.
- Repeated runs use separate files and retain the external exit status.

## Report template

- Question and prediction:
- Commit or implementation identifier:
- Java version, machine, OS/WSL, input filesystem, JVM options:
- Input generation command, seed, count, bytes, and maximum record size:
- Expected output and correctness checks:
- Timing boundaries, run order, warm-up/cache policy, and number of repetitions:
- Sampler requested interval, actual sample count, and any large timing gaps:
- Profiling enabled or disabled, and recording settings:

| Approach / run | Input bytes | Heap limit | Processing time | Process elapsed time | Peak process RSS (KB) | Max sampled heap (bytes) | Correct? / status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Add measured data here | | | | | | | |

- Median and range across successful, correct repetitions:
- Observation from heap samples and JFR:
- Explanation or hypothesis:
- Monitoring overhead and other limitations:
- Failures and incomplete measurements:
- Next experiment:

Keep small reports and CSVs in `results/`. For long runs, retain a small summary
in Git and keep large raw CSV recordings local, for example under the ignored
`data/generated/` directory. JFR and heap dumps are already ignored. Do not
fabricate results for unfinished approaches.

Treat these as end-to-end learning experiments. For precise JVM microbenchmarks
later, study a dedicated benchmarking harness and its pitfalls.

## References

- [Java heap management API](https://docs.oracle.com/en/java/javase/21/docs/api/java.management/java/lang/management/MemoryMXBean.html)
- [JFR command and event filtering](https://docs.oracle.com/en/java/javase/21/docs/specs/man/jfr.html)
- [Java launcher and recording options](https://docs.oracle.com/en/java/javase/21/docs/specs/man/java.html)
- [GNU time documentation](https://www.gnu.org/software/time/manual)

[Experiment index](README.md)
