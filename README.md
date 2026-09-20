# Java Learning Lab

Learn Java fundamentals and DSA, then apply them to practical experiments
in file processing, large JSON payloads, memory use, and concurrency.

This repository starts with an empty source tree and a learning plan.
Topic guides contain exercises and checkpoints; implementations and test
cases will be added as you learn.

## Start here

1. Read the [roadmap and first exercise](ROADMAP.md).
2. Write your implementation and matching tests.
3. Update [PROGRESS.md](PROGRESS.md) with the result and tests.

## Learning tracks

| Track | Contents |
| --- | --- |
| [Java fundamentals](docs/fundamentals/README.md) | Language, OOP, collections, generics, streams, I/O, concurrency, JVM |
| [DSA](docs/dsa/README.md) | Complexity, arrays, hashing, searching, sorting, trees, heaps, graphs, dynamic programming |
| [Practical experiments](docs/experiments/README.md) | File/JSON processing, loops and streams, batching, queues, caching, retries |

Larger standalone projects will live in separate repositories.

## Setup

Use JDK 21 and Maven 3.8.7 or newer. From the repository root:

```bash
java -version
mvn -version
mvn test
```

The initial build has no Java classes or tests, so Maven reports no tests to
run. Once you write the first exercise, the same command compiles and tests it.
The first build needs Maven repository access unless dependencies and plugins
are already cached. There is no application entry point yet.

## Layout

```text
README.md / ROADMAP.md / PROGRESS.md  Navigation, plan, and progress
pom.xml                             One Java 21 / JUnit Maven build
docs/fundamentals/                   Java concept guides
docs/dsa/                            Problem-solving guides
docs/experiments/                    Experiment plans and measurement guide
src/main/java/learning/              Your implementations, organized by topic
src/test/java/learning/              Your matching test packages
src/test/resources/                  Small sample inputs
data/generated/                     Large local datasets, ignored by Git
results/                            Small measurement reports
```

Empty source folders use .gitkeep so the proposed structure survives a Git
clone. Add test packages as you implement topics. Match package names to
directories, for example `learning.fundamentals.basics`.

## Learning workflow

- Attempt one exercise at a time and state input/output behavior first.
- Test normal, boundary, and invalid inputs relevant to the contract.
- Explain correctness and time/space tradeoffs in the topic guide.

For experiments, verify equivalent outputs before comparing performance.
Use the [measurement guide](docs/experiments/measurement-guide.md).

## GitHub habits

Review `git status` and `git diff` before staging one coherent change.
Commit source, tests, small fixtures, guides, generators, and small reports.
Generated datasets, build output, profiling dumps, and local editor settings
are ignored. Add automatic tests on GitHub once the local workflow is familiar.

## References

- [Official Java learning guides](https://dev.java/learn/)
- [Maven standard directory layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout)
- [Jackson incremental JSON parser](https://github.com/FasterXML/jackson-core)
