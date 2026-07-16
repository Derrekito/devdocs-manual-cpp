# std::stack

`std::stack` is a container **adaptor**: a thin wrapper that restricts
another container (a `std::deque` by default) to a LIFO interface —
push and pop only from one end, called the top. Reach for it when you
want "this is a stack" enforced by the type instead of using a
`vector` and promising yourself to only touch `back()`.

```cpp skip
std::stack<int> s;                       // wraps a std::deque<int>
std::stack<int, std::vector<int>> s2;    // or another SequenceContainer
s.push(x);                                // push onto the top
s.emplace(args...);                       // in place on top    (since C++11)
s.top();                                  // peek at the top element
s.pop();                                  // remove the top, returns void
s.empty(); s.size();
```

### Template parameters

- **T** — stored element type. Undefined behavior if it differs from
  `Container::value_type`.
- **Container** — the underlying container. Must be a SequenceContainer
  providing `back()`, `push_back()`, and `pop_back()` — `std::vector`
  (including `std::vector<bool>`), `std::deque`, and `std::list` all
  qualify. Defaults to `std::deque`.

### Guarantees and costs

- Every `stack` operation forwards directly to the underlying
  container: `push` calls `push_back`, `pop` calls `pop_back`, `top`
  calls `back`. Its cost is exactly whatever the chosen container
  charges for those calls.
- `stack` adds no iterators of its own — there's nothing to invalidate
  beyond what the underlying container's rules already say about
  `push_back`/`pop_back`.

### Gotchas

- No iteration or indexing by design — a stack hides the underlying
  container on purpose. Need to scan the contents? Use the underlying
  container (`vector`/`deque`) directly instead.
- `pop()` returns `void` — it only removes. Read `top()` before
  calling `pop()` if you need the value (the split keeps `pop`
  exception-safe).
- Swapping the default `deque` for a `vector` is common when you don't
  need the double-ended growth a deque offers and want tighter memory
  locality.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <stack>

int main()
{
    std::stack<int> s;

    s.push(1);
    s.push(2);
    s.push(3);

    assert(s.top() == 3);

    for (; !s.empty(); s.pop())
        std::cout << s.top() << ' ';
    std::cout << '\n';
}
```

```text
3 2 1
```

### Reference

```cpp skip
template<
    class T,
    class Container = std::deque<T>
> class stack;
```

Member functions, condensed:

- **(constructor)**, **(destructor)**, **operator=** — lifetime and
  assignment.
- **top** — accesses the top element.
- **empty / size** — capacity queries.
- **push** — inserts at the top; **push_range** (C++23) inserts a
  range; **emplace** (C++11) constructs in place at the top.
- **pop** — removes the top element.
- **swap** (C++11) — swaps contents with another stack.

Non-member: lexicographic comparison operators (C++20); `std::swap`
(C++11); `std::uses_allocator<std::stack>` (C++11) specializes the
allocator trait.

Feature-test macro `__cpp_lib_containers_ranges` (`202202L`, C++23)
marks ranges-based construction and insertion.

Defect report: LWG 307 (C++98) — `Container` could not be
`std::vector<bool>`; now allowed.

### See also

- **vector** — common alternative underlying container
- **deque** — the default underlying container

---
*Source: https://en.cppreference.com/w/cpp/container/stack*
