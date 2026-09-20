# Experiment data

Put large generated inputs in generated/. Git ignores their contents.
Commit small representative fixtures under src/test/resources/ instead.

When implementing a generator, record its command, seed, record count,
approximate byte size, and maximum record size in the experiment guide.
Generate records incrementally so dataset creation does not itself require
retaining the entire dataset in memory. No generators exist yet.
