# Producer-consumer processing

Status: planned; no implementation or measurements yet.

## Prerequisites

Executors, futures, interruption, queues, and a correct sequential processor.

## Question and task

Read records and process them with workers, deliberately making processing slower than reading.

## Approaches

1. Process sequentially.
2. Use concurrent workers with an unbounded queue on controlled small inputs.
3. Use concurrent workers with a bounded queue.

## Correctness checks

- All records accounted for.
- Slow consumers and full queues.
- Producer and worker failures.
- Cancellation and shutdown without indefinite blocking.
- Ordering requirements explicitly stated.

## Measurements

Record worker count, queue capacity, queue depth over time, memory, and throughput. Keep task cost comparable.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

A bounded queue applies backpressure; it must be paired with failure propagation and shutdown so producers do not remain blocked.

## Implementation and findings

Code location: `src/main/java/learning/experiments/producerconsumer/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
