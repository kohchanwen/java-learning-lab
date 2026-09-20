# File processing and memory

Status: planned; no implementation or measurements yet.

## Prerequisites

Methods, exceptions, collections, and basic I/O.

## Question and task

Count transaction rows and sum amounts in a UTF-8 text file.

## Approaches

1. Load the entire file.
2. Read and process one line at a time.
3. Later, read fixed-size chunks with correct record and character boundary handling.

## Correctness checks

- Empty input and missing final newline.
- Malformed rows and stated failure policy.
- Multibyte characters crossing buffer boundaries.
- One unusually long line.
- Identical count and sum for every approach.

## Measurements

Start with roughly 1 MB and 100 MB generated inputs; add a larger size only as local resources allow. Compare under the same heap limit.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

Incremental processing retains less input, but its memory still depends on the longest record, buffers, and accumulated output.

## Implementation and findings

Code location: `src/main/java/learning/experiments/fileprocessing/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
