# Batch size tradeoffs

Status: planned; no implementation or measurements yet.

## Prerequisites

Collections, generics, exceptions, and a sequential processor.

## Question and task

Send records to a simulated destination with a fixed per-call cost.

## Approaches

1. Send one record per call.
2. Send batches of 100.
3. Send batches of 1,000.

## Correctness checks

- Empty input and final partial batch.
- Exactly one full batch.
- Destination failure and stated retry behavior.
- Same records delivered without accidental loss or duplication.

## Measurements

Measure destination call count, retained batch size, elapsed time, and time to first completed batch. Keep the simulated cost fixed.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

Larger batches may improve throughput while increasing memory, initial latency, and work affected by one failure.

## Implementation and findings

Code location: `src/main/java/learning/experiments/batching/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
