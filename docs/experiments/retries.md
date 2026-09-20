# Timeouts, retries, and duplicate prevention

Status: planned; no implementation or measurements yet.

## Prerequisites

Exceptions, test doubles, and task cancellation.

## Question and task

Call a simulated service that sometimes fails temporarily, including a completed operation whose response is lost.

## Approaches

1. One attempt with a timeout.
2. A bounded number of attempts with increasing delays.
3. Retry using an operation identifier to prevent duplicate side effects.

## Correctness checks

- Temporary versus permanent failure.
- Attempt and overall time limits.
- Success followed by a lost response.
- Cancellation while waiting.
- No duplicate business effect when the operation is retried.

## Measurements

Use injected failure schedules and a controllable delay/clock in correctness tests. Record attempts and business effects separately.

Follow the [measurement guide](measurement-guide.md). Store small reports in
`results/` and generated large inputs in `data/generated/`.

## Completion checkpoint

A timeout does not prove the operation failed remotely. Retrying safely requires understanding side effects and idempotency.

## Implementation and findings

Code location: `src/main/java/learning/experiments/retries/`.
Create the matching test package when implementing the experiment.

- Prediction:
- How to run (add exact commands once implemented):
- Correctness evidence:
- Results and environment:
- Interpretation and limitations:
- Next question:

[Experiment index](README.md) · [Learning roadmap](../../ROADMAP.md)
