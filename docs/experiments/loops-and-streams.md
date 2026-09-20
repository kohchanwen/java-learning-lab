# Loops and Java streams

Status: planned; no implementation or measurements yet.

## Prerequisites

Collections, lambdas, and Stream API; concurrency before the parallel variant.

## Question and task

Filter orders, transform values, and calculate an identical summary.

## Approaches

1. Use an explicit loop.
2. Use a sequential Stream pipeline.
3. Later, compare a parallel stream with a workload that justifies the investigation.

## Correctness checks

- Empty input and duplicate values.
- Ordering where the output contract requires it.
- Equivalent aggregates and short-circuit behavior.
- No unsafe shared mutable state.

## Measurements

Discuss readability and retained collections first. Record warm-up and repeated runs if comparing speed.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

Stream<T> processing and file I/O streams are different abstractions; neither syntax nor parallelism guarantees better performance.

## Implementation and findings

Code location: `src/main/java/learning/experiments/loopsstreams/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
