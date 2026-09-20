# Caching tradeoffs

Status: planned; no implementation or measurements yet.

## Prerequisites

Maps, equality, date/time concepts, and basic testing.

## Question and task

Look up values from a simulated slow service under repeated-key and mostly-unique-key workloads.

## Approaches

1. No cache.
2. Unbounded map.
3. Cache with a size limit.
4. Later, expiry with a controllable clock.

## Correctness checks

- Cache hits and misses.
- Eviction at capacity.
- Expired values.
- Underlying values change.
- Defined handling of lookup failures.

## Measurements

Record hit rate, external calls, retained entries, and elapsed time for each workload. Define acceptable staleness before comparing outputs.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

Caching trades memory and freshness for reduced repeated work; unique-key workloads can provide little benefit.

## Implementation and findings

Code location: `src/main/java/learning/experiments/caching/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
