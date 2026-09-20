# Data structures and algorithms

Follow the topic order below.

| Topic | Guide |
| --- | --- |
| 1. Arrays and strings | [arrays-and-strings](arrays-and-strings.md) |
| 2. Hashing | [hashing](hashing.md) |
| 3. Linked lists | [linked-lists](linked-lists.md) |
| 4. Stacks and queues | [stacks-and-queues](stacks-and-queues.md) |
| 5. Searching | [searching](searching.md) |
| 6. Sorting | [sorting](sorting.md) |
| 7. Recursion and backtracking | [recursion-and-backtracking](recursion-and-backtracking.md) |
| 8. Trees | [trees](trees.md) |
| 9. Heaps | [heaps](heaps.md) |
| 10. Graphs | [graphs](graphs.md) |
| 11. Dynamic programming | [dynamic-programming](dynamic-programming.md) |

## Complexity reference

Use these tables when choosing a data structure or explaining a solution.
Each exercise should still state its own time and space complexity.

- `n`: number of elements; for strings, the number of UTF-16 code units.
- `h`: tree height; `w`: maximum tree width; `V` / `E`: graph vertices / edges.
- Expected costs assume well-distributed hashes. Amortized costs spread an
  occasional expensive operation, such as resizing, across a sequence of operations.
- Unless stated otherwise, comparisons, hashing, and equality checks cost O(1).
  Long strings or custom comparison/hash functions can add their own cost.
- Algorithm space means auxiliary memory, including recursion, but excluding
  input storage and returned output unless the row says otherwise.

### Data structures and Java collections

| Structure | Access / lookup | Search by value | Insert / add | Remove | Important condition |
| --- | --- | --- | --- | --- | --- |
| Array | Index: O(1) | Unsorted: O(n) | Shift elements: O(n) | Shift elements: O(n) | Fixed length; writing an existing slot is O(1); growing requires a new array |
| `String` | `charAt`: O(1) | Single-character scan: O(n) | Creates new text: O(n + m) for concatenating lengths n and m | Copying remaining text: O(n) | Immutable; substring-pattern search has separate algorithm-dependent costs |
| `ArrayList` | Index: O(1) | `contains`: O(n) | Append: amortized O(1); indexed insertion: O(n) | Indexed removal: O(n); last element: O(1) | A resizing append can take O(n) |
| `LinkedList` | Index: O(n); ends: O(1) | `contains`: O(n) | Ends: O(1); arbitrary index: O(n) | Ends: O(1); by index/value: O(n) | Inserting/removing through an already-positioned iterator avoids the traversal cost |
| `HashMap` | `get` / `containsKey`: expected O(1) | `containsValue`: O(n)* | `put`: expected amortized O(1) | By key: expected O(1) | Key lookup and value search are different operations |
| `HashSet` | `contains`: expected O(1) | Membership uses hashing | `add`: expected amortized O(1) | Expected O(1) | Collision behavior follows its backing hash map |
| `LinkedHashMap` / `LinkedHashSet` | Key/membership: expected O(1) | Map value search: O(n) | Expected amortized O(1) | Key/element: expected O(1) | Maintains encounter order; iteration is O(n) |
| `ArrayDeque` as stack/queue | Peek at either end: O(1) | `contains`: O(n) | Either end: amortized O(1) | Either end: O(1); by value: O(n) | Resizing insertion can take O(n); use `push`/`pop` or `offer`/`poll` |
| `PriorityQueue` (binary heap) | `peek`: O(1) | `contains`: O(n) | `offer`: amortized O(log n) including growth | `poll`: O(log n); `remove(value)`: O(n) | Occasional backing-array growth is O(n); iteration is not sorted |
| Unbalanced binary search tree | By key: O(h) | By key: O(h) | O(h) | O(h) | Height can become n, making these operations O(n) |
| `TreeMap` / `TreeSet` (balanced tree) | Key/membership: O(log n) | Map value search: O(n) | O(log n) | Key/element: O(log n) | Logarithmic key operations are worst-case guarantees |

*Hash tables are assumed to have capacity proportional to their entry count.
A `HashMap` value scan or full iteration is more precisely O(capacity + n).
Hash-based O(1) costs are not worst-case guarantees: pathological collisions can
make key lookup O(n). Modern Java can use tree-shaped buckets to improve many
collision cases, but arbitrary keys should not be assumed to guarantee O(log n).

The structures above store O(n) elements/characters in ordinary use. Backing
array/table capacity and object overhead affect actual memory; a collection can
retain a large backing array after elements are removed. Stack and queue costs
always depend on their implementation, not just the `Stack` or `Queue` concept.

Java implementation references: [HashMap](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/HashMap.html),
[ArrayList](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayList.html),
[ArrayDeque](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/ArrayDeque.html),
[PriorityQueue](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/PriorityQueue.html),
and [TreeMap](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/TreeMap.html).

### Searching and sorting

These are standard implementations, not promises about every Java library sort.
For quicksort, the expected bound assumes randomized pivots or a suitable input
model; the recursion-space figures describe the usual two-recursive-call version.

| Algorithm | Average / expected time | Worst-case time | Auxiliary space | Condition |
| --- | --- | --- | --- | --- |
| Linear search | O(n) | O(n) | O(1) | No sorting required |
| Iterative binary search | O(log n) | O(log n) | O(1) | Sorted data with O(1) indexed access |
| Insertion sort | O(n²) | O(n²) | O(1) | Best case O(n) on already sorted input |
| Selection sort | O(n²) | O(n²) | O(1) | Repeatedly select the next smallest element |
| Bubble sort with early exit | O(n²) | O(n²) | O(1) | Best case O(n) when no swaps are needed |
| Array merge sort | O(n log n) | O(n log n) | O(n) | Uses a merge buffer |
| Quicksort | O(n log n) | O(n²) | Expected O(log n); worst O(n) | Partition balance controls recursion depth |
| Heapsort | O(n log n) | O(n log n) | O(1) | In-place array implementation |

Classic sorting and graph bounds are also summarized in the
[Princeton algorithms reference](https://algs4.cs.princeton.edu/cheatsheet/).

### Patterns across the DSA topics

A topic such as backtracking or dynamic programming has no single universal
complexity. The representative examples below state the relevant assumptions.

| Topic / example | Time | Auxiliary space | Condition |
| --- | --- | --- | --- |
| Arrays: two pointers | O(n) | O(1) | Each pointer moves through the input at most once |
| Arrays: fixed-size sliding-window sum | O(n) | O(1) | Update a running sum instead of rescanning each window |
| Arrays: prefix sums | Build O(n); range-sum query O(1) | O(n) for the prefix table | Input does not change between queries |
| Hashing: two sum with a map | Expected O(n) | O(n) | One pass with expected O(1) key operations |
| Linked lists: iterative reversal | O(n) | O(1) | Rewire links in place |
| Stacks: balanced brackets | O(n) | O(n) | Stack can hold all opening brackets |
| Stacks: monotonic stack | O(n) | O(n) | Each element is pushed and popped at most once |
| Queues: ticket simulation | O(T), where T is tickets served before stopping | O(n) | At most n people remain in the queue |
| Heaps: retain top k values | O(n log k), for 2 ≤ k ≤ n | O(k) | k = 1 takes O(n); sorted output adds O(k log k) |
| Trees: DFS traversal | O(n) | O(h) | Recursive traversal; a skewed tree has h = n |
| Trees: BFS traversal | O(n) | O(w) | Queue size depends on the widest level |
| Graphs: BFS / DFS / topological sort | O(V + E) | O(V) | Adjacency lists; enqueue/push each vertex once or mark it when discovered |
| Backtracking: enumerate all subsets | O(n × 2^n) | O(n) | Includes copying each produced subset; storing all output needs O(n × 2^n) space |
| Backtracking: enumerate all permutations | O(n × n!) | O(n) | Distinct elements; storing all output needs O(n × n!) space |
| DP: climbing stairs / house robber | O(n) | O(n), or O(1) with rolling state | Each state does constant work |
| DP: minimum-coin change | O(A × c) | O(A) | A is target amount, c is number of positive coin denominations |

For other DP problems, count the states and the work needed for each state.
Include recursion depth for memoized solutions. For graph storage itself,
adjacency lists need O(V + E) space; an adjacency matrix needs O(V²), and scanning
matrix neighbors changes traversal time to O(V²).

[Learning roadmap](../../ROADMAP.md)
