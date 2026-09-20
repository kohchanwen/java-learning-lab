# Large JSON processing

Status: planned; no implementation or measurements yet.

## Prerequisites

The file-processing experiment, collections, and exceptions.

## Question and task

Count transactions and sum amounts from one JSON array.

## Approaches

1. Deserialize the whole array into a list.
2. Build a whole-document tree.
3. Use an incremental parser and bind one record at a time.
4. Later, consume tokens and retain only required fields.

## Correctness checks

- Nested objects and arbitrary field order.
- Missing optional and required fields.
- Malformed or truncated JSON.
- One unusually large record.
- Equivalent outputs across all implementations.

## Measurements

Start with count and sum, then group by customer to observe state growth with distinct customers. Generate input incrementally so the generator does not require a full in-memory dataset.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

An InputStream parameter does not prevent a parser from creating a full list or tree. Streaming also loses its memory advantage if every result is retained. JSON arrays are not newline-delimited JSON.

## Implementation and findings

Code location: `src/main/java/learning/experiments/jsonprocessing/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
