# std::queue

`std::queue` is a container **adaptor**: it isn't a container itself,
but a thin wrapper that restricts another container (a `std::deque` by
default) to a FIFO interface — push at the back, pop from the front,
nothing else. Reach for it when you want "this is a queue, not a
general sequence" enforced by the type, so the compiler stops you from
accidentally indexing or iterating it.

```cpp skip
std::queue<int> q;                       // wraps a std::deque<int>
std::queue<int, std::list<int>> q2;      // or another SequenceContainer
q.push(x);                                // add at the back
q.emplace(args...);                       // in place at the back (since C++11)
q.front(); q.back();                      // peek at either end
q.pop();                                  // remove from the front, returns void
q.empty(); q.size();
```

### Template parameters

- **T** — stored element type. Undefined behavior if it differs from
  `Container::value_type`.
- **Container** — the underlying container. Must be a SequenceContainer
  providing `back()`, `front()`, `push_back()`, and `pop_front()` —
  `std::deque` and `std::list` qualify; `std::vector` does not (it has
  no `pop_front()`).

### Guarantees and costs

- Every `queue` operation forwards directly to the underlying
  container: `push` calls `push_back`, `pop` calls `pop_front`,
  `front`/`back` call the container's `front`/`back`. Its cost is
  exactly whatever the chosen container charges for those calls.
- `queue` adds no iterators of its own — there's nothing to invalidate
  beyond what the underlying container's rules already say about
  `push_back`/`pop_front`.

### Gotchas

- No iteration or random access by design — the whole point of the
  adaptor is to expose only `front`, `back`, `push`, `pop`, `empty`,
  `size`. Need to scan the contents? Use the underlying container
  (`deque`) directly instead.
- `pop()` returns `void` — it only removes. Read `front()` (or
  `back()`) before calling `pop()` if you need the value.
- `std::vector` can't be used as the underlying container: it has no
  `pop_front()`, one of the operations `queue` requires.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <queue>

int main()
{
    std::queue<int> q;

    q.push(0);
    q.push(1);
    q.push(2);

    assert(q.front() == 0);
    assert(q.back() == 2);

    q.pop();   // removes 0

    for (; !q.empty(); q.pop())
        std::cout << q.front() << ' ';
    std::cout << '\n';
}
```

```text
1 2
```

### Reference

```cpp skip
template<
    class T,
    class Container = std::deque<T>
> class queue;
```

Member functions, condensed:

- **(constructor)**, **(destructor)**, **operator=** — lifetime and
  assignment.
- **front / back** — access the first / last element.
- **empty / size** — capacity queries.
- **push** — inserts at the end; **push_range** (C++23) inserts a
  range; **emplace** (C++11) constructs in place at the end.
- **pop** — removes the first element.
- **swap** (C++11) — swaps contents with another queue.

Non-member: lexicographic comparison operators (C++20); `std::swap`
(C++11); `std::uses_allocator<std::queue>` (C++11) specializes the
allocator trait.

Feature-test macro `__cpp_lib_containers_ranges` (`202202L`, C++23)
marks ranges-based construction and insertion.

Defect report: LWG 307 (C++98) added support for containers using
proxy reference types (such as `std::vector<bool>`-like types with a
`pop_front()`) in place of `(const) value_type&`; the change to
`queue` itself is only for consistency with `stack` and
`priority_queue`, which gained real `std::vector<bool>` support from
the same DR.

### See also

- **deque** — the default underlying container
- **stack** — the LIFO counterpart adaptor

---
*Source: https://en.cppreference.com/w/cpp/container/queue*
