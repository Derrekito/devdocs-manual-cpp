# std::inserter

Wraps a container and a position in an output iterator that calls
`c.insert(pos, x)` for each write, then moves `pos` past the newly
inserted element. Reach for it where **back_inserter** doesn't apply:
containers without `push_back` (`set`, `map`, `multiset`, `multimap`),
or inserting into the middle of a sequence instead of always
appending at the end.

```cpp skip
std::inserter(c, i)   // c.insert(i, x) each write; i advances  // (until C++20)
std::inserter(c, i)   // same, constexpr                        // (since C++20)
```

### What you provide

- **c** — a container with an `insert(iterator, value)` member: any
  sequence container, or an associative container (`set`, `map`,
  `multiset`, `multimap`).
- **i** — iterator marking the insertion point. For a sequence
  container this is where each element lands; for an associative
  container it is only a positional hint, since the actual position is
  determined by the container's order.

### Guarantees and costs

- Each write inserts one element at (or, for associative containers,
  near) the tracked position, then advances that internal position
  past what was just inserted — so repeated writes lay elements down
  in sequence instead of always hitting the original spot.
- Cost is whatever `c.insert` costs there: near O(1) amortized for a
  `set`/`map` insertion with a correct hint, but O(N) per call for
  `vector`/`deque` since later elements must shift.
- Since C++20 the iterator type is deduced as
  `ranges::iterator_t<Container>` rather than `Container::iterator`
  (matters for iterator adaptors and views); `constexpr` since C++20.

### Gotchas

- On a `vector` or `deque`, each insertion can shift every following
  element — repeated use is O(N) per call, O(N²) overall. If you only
  need to append, `back_inserter` is far cheaper.
- The type of `i` is tied to `Container` (fixed by LWG 561; earlier
  wording left it independent) — pass an iterator from `c` itself.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <set>
#include <vector>

int main()
{
    std::multiset<int> s{1, 2, 3};

    // inserter is commonly used with associative containers
    std::fill_n(std::inserter(s, s.end()), 5, 2);

    for (int n : s)
        std::cout << n << ' ';
    std::cout << '\n';

    std::vector<int> d{100, 200, 300};
    std::vector<int> v{1, 2, 3, 4, 5};

    // in a sequence container, the insertion point advances because
    // each write updates the tracked position
    std::copy(d.begin(), d.end(), std::inserter(v, std::next(v.begin())));

    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
1 2 2 2 2 2 2 3 
1 100 200 300 2 3 4 5
```

### Reference

```cpp skip
template< class Container >
std::insert_iterator<Container>
    inserter( Container& c, typename Container::iterator i );  // (until C++20)
template< class Container >
constexpr std::insert_iterator<Container>
    inserter( Container& c, ranges::iterator_t<Container> i );  // (since C++20)
```

Equivalent to `return std::insert_iterator<Container>(c, i);`.

### See also

- **insert_iterator** — the iterator type `inserter` builds
- **back_inserter** — append via `push_back`, no position argument
- **front_inserter** — prepend via `push_front`

---
*Source: https://en.cppreference.com/w/cpp/iterator/inserter*
