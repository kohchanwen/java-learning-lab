# Concurrency

Status: not started.

## Prerequisites

OOP, exceptions, collections, and sequential I/O.

## Learning sequence

1. Threads and task lifecycles.
2. race conditions.
3. visibility versus atomicity.
4. synchronized and atomics.
5. executors and futures.
6. interruption.
7. bounded queues.
8. CompletableFuture.
9. virtual threads.

## Exercise

Reproduce a lost update, fix it, then build workers that process a bounded queue and shut down after success or failure.

Put code in `src/main/java/learning/fundamentals/concurrency/` and matching tests
under `src/test/java/learning/fundamentals/concurrency/` as you implement it.

## Completion checkpoint

Explain shared-state protection, cancellation, queue bounds, and why more threads need not improve throughput.

## Implementation notes

- Implementation:
- Tests:
- Tradeoffs:

[Topic index](README.md) · [Learning roadmap](../../ROADMAP.md)
