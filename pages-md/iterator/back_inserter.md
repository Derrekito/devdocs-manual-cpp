# std::back_inserter

Wraps a container in an output iterator that calls `push_back` instead
of writing through `*it`. This is the fix for the classic crash where
`std::copy(src.begin(), src.end(), dst.begin())` writes past the end
of an empty or too-small `dst`: hand it `back_inserter(dst)` and each
write appends a new element instead of overwriting one that isn't
there.

```cpp skip
std::back_inserter(c)  // wraps c; writes call c.push_back(x)  // (constexpr since C++20)
```

### What you provide

- **c** — a container with a `push_back` member: `vector`, `deque`,
  `list`, `string`. Not `array` or `forward_list` — no `push_back`.

### Guarantees and costs

- Every assignment through the returned iterator calls
  `c.push_back(x)`; dereference and increment are no-ops that just
  return the iterator itself, so any write-through-iterator algorithm
  works unmodified.
- Cost is whatever `c.push_back` costs — amortized O(1) for `vector`
  and `deque`.
- `constexpr` since C++20.

### Gotchas

- It appends to whatever is already in `c`; it does not clear `c`
  first. Reusing a non-empty container adds to it rather than
  overwriting it.
- Works only for containers with `push_back` — there's no separate
  compile-time check, so using it on an unsupported container fails
  inside the call, not at the `back_inserter` call site itself.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::fill_n(std::back_inserter(v), 3, -1);
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
1 2 3 4 5 6 7 8 9 10 -1 -1 -1
```

### Reference

```cpp skip
template< class Container >
std::back_insert_iterator<Container> back_inserter( Container& c );  // (constexpr since C++20)
```

Equivalent to `return std::back_insert_iterator<Container>(c);` — the
container type is deduced from the argument.

### See also

- **back_insert_iterator** — the iterator type `back_inserter` builds
- **front_inserter** — same idea, but prepends via `push_front`
- **inserter** — insert at an arbitrary position, for sets/maps or
  mid-sequence insertion

---
*Source: https://en.cppreference.com/w/cpp/iterator/back_inserter*
