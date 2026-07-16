# std::priority_queue

`std::priority_queue` is a container adaptor that keeps its underlying
container (`std::vector` by default) heap-ordered, giving O(1) lookup
of the largest element at the cost of O(log n) insertion and removal.
Supply a different `Compare` (e.g. `std::greater<T>`) to get the
smallest element on top instead. Reach for it whenever you need "give
me the current max/min, repeatedly, while things keep getting added" —
scheduling, Dijkstra/A*, merging sorted runs, top-K — rather than
resorting a vector by hand.

```cpp skip
std::priority_queue<int> pq;                     // max-heap
pq.push(x);                                       // O(log n)
pq.emplace(args...);                              // O(log n)  (since C++11)
pq.top();                                         // O(1), largest element
pq.pop();                                         // O(log n), returns void

// min-heap: smallest on top
std::priority_queue<int, std::vector<int>, std::greater<int>> minpq;

pq = std::priority_queue<int>(first, last);       // heapify a range, O(n)
```

### Template parameters

- **T** — stored element type. Undefined behavior if it differs from
  `Container::value_type`.
- **Container** — the underlying container: a SequenceContainer with
  LegacyRandomAccessIterator iterators, providing `front()`,
  `push_back()`, `pop_back()`. `std::vector` (including
  `std::vector<bool>`) and `std::deque` qualify.
- **Compare** — a strict weak ordering; returns `true` when its first
  argument comes *before* its second. Since the queue outputs the
  largest element first, the element that "comes before" by `Compare`
  is the one output *last* — `top()` holds the element `Compare`
  considers last, not first.

### Guarantees and costs

- `top()`: O(1) constant.
- `push()`/`emplace()`/`pop()`: O(log n), since each has to restore
  the heap property.
- Constructing from an iterator range heapifies the whole range in one
  O(n) pass, cheaper than pushing elements one at a time.
- Working with a `priority_queue` behaves like managing a heap inside
  a random-access container, minus the risk of accidentally breaking
  the heap invariant by mutating it directly.

### Gotchas

- The `Compare` sense is inverted from what feels natural: the default
  `std::less` gives a **max**-heap; `std::greater` gives a **min**-heap.
  A custom comparator returning `a.priority > b.priority` puts the
  *smallest* `priority` on top.
- `pop()` returns `void` — it only removes. Read `top()` before
  calling `pop()` if you need the value.
- No iteration, indexing, or decrease-key: you can't find or update an
  arbitrary element. Needing that means tracking indices separately or
  reaching for a different structure.

### Example

```cpp
#include <iostream>
#include <queue>

int main()
{
    std::priority_queue<int> pq;
    for (int x : {3, 1, 4, 1, 5})
        pq.push(x);

    while (!pq.empty())
    {
        std::cout << pq.top() << ' ';
        pq.pop();
    }
    std::cout << '\n';
}
```

```text
5 4 3 1 1
```

### Reference

```cpp skip
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

Member functions, condensed:

- **(constructor)**, **(destructor)**, **operator=** — lifetime and
  assignment.
- **top** — accesses the top element.
- **empty / size** — capacity queries.
- **push** — inserts and re-heapifies; **push_range** (C++23) inserts
  a range; **emplace** (C++11) constructs in place and re-heapifies.
- **pop** — removes the top element.
- **swap** (C++11) — swaps contents with another priority_queue.

Non-member: `std::swap` (C++11) specializes the swap algorithm.
`std::uses_allocator<std::priority_queue>` (C++11) specializes the
allocator trait.

Feature-test macro `__cpp_lib_containers_ranges` (`202202L`, C++23)
marks ranges-aware construction and insertion.

Defect reports: LWG 307 (C++98) — `Container` could not be
`std::vector<bool>`; now allowed. LWG 2684 (C++98) —
`priority_queue` took a comparator but had no member typedef for it;
`value_compare` was added.

### See also

- **vector** — the default underlying container
- **deque** — an alternative underlying container
- **queue** — the FIFO adaptor counterpart

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue*
