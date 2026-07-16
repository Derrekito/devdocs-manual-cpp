# std::multimap

`std::multimap` is `std::map` with one difference: it allows multiple
entries to share the same key. That difference removes `operator[]` —
there is no single value to return for a key that may have several —
so lookups go through `find`, `count`, or, for all the entries at
once, `equal_range`. Keys stay sorted by `Compare`; search, insertion,
and removal are all O(log n).

```cpp skip
std::multimap<std::string, int> m;
std::multimap<std::string, int> m{{"a", 1}, {"a", 2}, {"b", 3}};

m.insert({"a", 4});                  // always inserts, never overwrites
m.emplace("a", 5);                   // construct in place              (C++11)

m.count("a");                        // number of entries with key "a"
m.find("a");                         // iterator to *one* matching entry
auto [lo, hi] = m.equal_range("a");  // iterator range over *all* of them
for (auto it = lo; it != hi; ++it)
    /* it->second */;

m.erase("a");                        // erases every entry with key "a"
```

### Guarantees and costs

- `find`, `count`, `insert`, `emplace`, `erase`, `equal_range`,
  `lower_bound`, `upper_bound`: O(log n).
- Iterators traverse keys in non-descending order (per `Compare`).
  Entries whose keys compare equivalent keep their relative insertion
  order (since C++11).
- `multimap` meets the requirements of Container,
  AllocatorAwareContainer, AssociativeContainer, and
  ReversibleContainer.
- `value_type` is `std::pair<const Key, T>`; equivalence between keys
  is defined by `!comp(a, b) && !comp(b, a)`, not `operator==`.
- `extract`/`merge` (C++17) move nodes out of or between containers
  without copying keys or values.

### Gotchas

- No `operator[]` — a duplicate-key container has no single value to
  hand back for a key. Use `insert`/`emplace` to add and
  `find`/`equal_range` to read.
- `find` returns *one* matching iterator, not all of them; iterate
  `equal_range` when a key may have more than one entry.
- `erase(key)` removes every entry with that key at once — pass an
  iterator instead if only one of several duplicates should go.
- `Compare` must impose a strict weak ordering; keys that don't
  compare less either way are treated as equivalent, not necessarily
  equal by `==`.

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::multimap<std::string, int> m;
    m.insert({"a", 1});
    m.insert({"a", 2});
    m.insert({"b", 3});

    auto [lo, hi] = m.equal_range("a");
    for (auto it = lo; it != hi; ++it)
        std::cout << it->first << " => " << it->second << '\n';
}
```

```text
a => 1
a => 2
```

### Reference

```cpp skip
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class multimap;  // (1)
namespace pmr {
    template<
        class Key,
        class T,
        class Compare = std::less<Key>
    > using multimap = std::multimap<Key, T, Compare,
        std::pmr::polymorphic_allocator<std::pair<const Key, T>>>;
}  // (2) (since C++17)
```

`Key` must be CopyConstructible (a defect report closed a gap in the
original wording that omitted this). `node_type` (C++17) is a
specialization of the node handle type shared with `map`.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `get_allocator`

  Iterators
  - `begin`/`cbegin`, `end`/`cend` (C++11)
  - `rbegin`/`crbegin`, `rend`/`crend` (C++11)

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert` — elements or nodes (C++17)
  - `insert_range` (C++23)
  - `emplace` (C++11), `emplace_hint` (C++11)
  - `erase`
  - `swap`
  - `extract` (C++17), `merge` (C++17)

  Lookup
  - `count`, `find`, `contains` (C++20)
  - `equal_range`, `lower_bound`, `upper_bound`

  Observers
  - `key_comp`, `value_comp`

Non-member: lexicographic `operator==`/`<=>` (C++20 replaces the
individual comparison operators with `<=>`), `std::swap`,
`std::erase_if` (C++20; unlike sequence containers, `multimap` has no
free-function `erase`). A `pmr::multimap` alias and deduction guides
(C++17) are also provided.

### See also

- **map** — same key-value container, but unique keys and `operator[]`
- **multiset** — same duplicate-key semantics, keys only, no mapped
  value
- **unordered_multimap** — same duplicate-key semantics, hashed
  instead of sorted

---
*Source: https://en.cppreference.com/w/cpp/container/multimap*
