# Learning roadmap

Start here and work through one exercise at a time. Every learning topic is
initially incomplete; record your attempts in [PROGRESS.md](PROGRESS.md).

## First exercise

Create `learning.fundamentals.basics.NumberStatistics` with two methods:

- `long sum(int[] numbers)`: return the sum; return 0 for an empty array.
- `int max(int[] numbers)`: return the largest value; reject an empty array.

Reject null input with IllegalArgumentException in both methods. Use loops
initially. Add matching JUnit tests in `src/test/java/learning/fundamentals/basics/`.
Test positive and negative values, one element, empty and null inputs, and a sum
larger than the int range. Explain why the sum uses long.

Run `mvn test` from the repository root. No solution or test implementation has
been supplied; writing both is the first learning task.

## Suggested first pass

Approximately five to seven hours per week; advance using checkpoints rather
than deadlines. DSA runs alongside the main track.

| Period | Main focus | Deliverable |
| --- | --- | --- |
| Weeks 1–2 | Language basics, references, testing | Small tested calculator |
| Weeks 3–4 | OOP, equality, collections | Checkout policies and custom sorting |
| Weeks 5–6 | Generics, lambdas, streams | Batch processor and order report |
| Weeks 7–8 | Exceptions and I/O | File-processing comparison |
| Weeks 9–10 | JSON | Full-load versus incremental comparison |
| Weeks 11–13 | Concurrency and coordination | Bounded producer-consumer experiment |
| Weeks 14–15 | JVM behavior and measurement | Reproducible performance/memory report |

You can begin practical I/O once methods, collections, and exceptions are
comfortable; you do not need to finish every topic first.

## Fundamentals checklist

- [ ] [Language basics](docs/fundamentals/language-basics.md)
- [ ] [Objects and references](docs/fundamentals/objects-and-references.md)
- [ ] [Strings and numbers](docs/fundamentals/strings-and-numbers.md)
- [ ] [Object-oriented programming](docs/fundamentals/oop.md)
- [ ] [Equality and immutability](docs/fundamentals/equality-and-immutability.md)
- [ ] [Exceptions and resource management](docs/fundamentals/exceptions.md)
- [ ] [Collections](docs/fundamentals/collections.md)
- [ ] [Generics](docs/fundamentals/generics.md)
- [ ] [Lambdas and functional interfaces](docs/fundamentals/lambdas.md)
- [ ] [Stream API](docs/fundamentals/streams.md)
- [ ] [Date and time](docs/fundamentals/date-time.md)
- [ ] [File I/O](docs/fundamentals/io.md)
- [ ] [Concurrency](docs/fundamentals/concurrency.md)
- [ ] [JVM and measurement](docs/fundamentals/jvm.md)
- [ ] [Testing and debugging](docs/fundamentals/testing-and-debugging.md)

## DSA checklist

- [ ] [Arrays and strings](docs/dsa/arrays-and-strings.md)
- [ ] [Hashing](docs/dsa/hashing.md)
- [ ] [Linked lists](docs/dsa/linked-lists.md)
- [ ] [Stacks and queues](docs/dsa/stacks-and-queues.md)
- [ ] [Searching](docs/dsa/searching.md)
- [ ] [Sorting](docs/dsa/sorting.md)
- [ ] [Recursion and backtracking](docs/dsa/recursion-and-backtracking.md)
- [ ] [Trees](docs/dsa/trees.md)
- [ ] [Heaps](docs/dsa/heaps.md)
- [ ] [Graphs](docs/dsa/graphs.md)
- [ ] [Dynamic programming](docs/dsa/dynamic-programming.md)

## Experiments checklist

- [ ] [File processing and memory](docs/experiments/file-processing.md)
- [ ] [Large JSON processing](docs/experiments/json-processing.md)
- [ ] [Loops and Java streams](docs/experiments/loops-and-streams.md)
- [ ] [Batch size tradeoffs](docs/experiments/batching.md)
- [ ] [Producer-consumer processing](docs/experiments/producer-consumer.md)
- [ ] [Caching tradeoffs](docs/experiments/caching.md)
- [ ] [Timeouts, retries, and duplicate prevention](docs/experiments/retries.md)

## Working rhythm

- Two short DSA sessions.
- One concept-and-exercise session.
- One longer experiment session.

## Completion checkpoint

Mark a topic complete when you can implement a small example, test the
important cases, and explain its behavior and tradeoffs.

Record the date, exercise, status, and tests in [PROGRESS.md](PROGRESS.md).
