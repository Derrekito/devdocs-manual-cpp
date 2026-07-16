# std::map

A sorted collection of key-value pairs with unique keys, kept in
ascending order by a comparison function (`std::less<Key>` by
default). Typically implemented as a red-black tree. Reach for it when
you need ordered iteration or range queries over keys; if you only need
fast lookup and don't care about order, `std::unordered_map` is
usually faster.

Search, insertion, and removal are all O(log n). Iterating the whole
map visits keys in ascending order.

```cpp skip
std::map<std::string, int> m;
std::map<std::string, int> m{{"a", 1}, {"b", 2}};

m["key"] = value;                    // insert-or-overwrite, default-constructs
m.insert({"key", value});            // insert only if absent
m.insert_or_assign("key", value);    // insert-or-overwrite, no default (C++17)
m.try_emplace("key", value);         // insert-if-absent, no default    (C++17)
m.emplace("key", value);             // construct in place

m.find("key");                       // iterator, or end() if absent
m.contains("key");                   // bool                            (C++20)
m.count("key");                      // 0 or 1 (keys are unique)
m.erase("key");                      // by key
m.erase(it);                         // by iterator

for (const auto& [k, v] : m) { ... } // visits keys in ascending order
```

### Guarantees and costs

- `find`, `count`, `contains`, `insert`, `emplace`, `erase` (by key or
  iterator), `lower_bound`, `upper_bound`: all O(log n).
- `operator[]`: O(log n); default-constructs `T` and inserts it if the
  key is absent, then returns a reference — this is a **write**
  operation and does not compile on a `const map`.
- Iterators, pointers, and references to existing elements are **never
  invalidated** by `insert`, `emplace`, or `erase`, except for the
  element actually erased. This holds even across insertions that
  would reallocate a `vector` — a map's nodes don't move.
- `erase(it)` returns an iterator to the element that followed the
  erased one, so it can be reused directly instead of separately
  advancing.
- `clear`, `operator=`, `swap`: `swap` invalidates no iterators (they
  now refer into the other container, except the end iterators, which
  swap too); `clear` and copy-assignment invalidate everything.
- Keys are immutable through the container: you cannot modify a key in
  place (doing so would break the ordering invariant), only erase and
  reinsert.

### Gotchas

- `operator[]` silently inserts a default-constructed value for a
  missing key — a lookup written as `m[k]` is actually a mutation.
  Use `find` or `contains` (C++20) for a read-only check, and
  `insert_or_assign`/`try_emplace` (C++17) when you want insert-or-
  update without the implicit default-construct-then-assign.
- Erasing while iterating with a raw `for` loop and `++it` after
  `erase(it)` skips an element or runs past `end()` — reassign `it`
  from `erase`'s return value instead, or use `std::erase_if` (C++20).
- Need duplicate keys? Use `std::multimap`. Don't need ordering?
  `std::unordered_map` gives average O(1) instead of O(log n).

### Example

```cpp
#include <iostream>
#include <map>
#include <string>

int main()
{
    std::map<std::string, int> m{{"CPU", 10}, {"GPU", 15}, {"RAM", 20}};

    m["CPU"] = 25;   // update existing
    m["SSD"] = 30;   // insert new

    m.erase("GPU");

    for (const auto& [key, value] : m)
        std::cout << '[' << key << "] = " << value << "; ";
    std::cout << '\n';
}
```

```text
[CPU] = 25; [RAM] = 20; [SSD] = 30; 
```

### Reference

```cpp skip
template<
    class Key,
    class T,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<std::pair<const Key, T>>
> class map;  // (1)
namespace pmr {
    template< class Key, class T, class Compare = std::less<Key> >
    using map = std::map<Key, T, Compare,
                          std::pmr::polymorphic_allocator<std::pair<const Key, T>>>;
}  // (2) (since C++17)
```

Two keys `a`, `b` are equivalent (treated as the same key) when
neither compares less than the other: `!comp(a, b) && !comp(b, a)` —
this is the uniqueness test the container uses, not `operator==`.
`value_type` is `std::pair<const Key, T>`; iterators are
LegacyBidirectionalIterator. `map` meets the requirements of
Container, AllocatorAwareContainer, AssociativeContainer, and
ReversibleContainer.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `get_allocator`

  Element access
  - `at` — bounds-checked access, throws `std::out_of_range`
  - `operator[]` — access or insert-default

  Iterators
  - `begin`/`cbegin`, `end`/`cend` (C++11)
  - `rbegin`/`crbegin`, `rend`/`crend` (C++11)

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `insert_or_assign` (C++17)
  - `emplace`, `emplace_hint` (C++11)
  - `try_emplace` (C++17)
  - `erase`
  - `swap`
  - `extract`, `merge` (C++17) — move nodes out of / between containers
    without copying or reallocating keys/values

  Lookup
  - `count`, `find`, `contains` (C++20), `equal_range`
  - `lower_bound`, `upper_bound`

  Observers
  - `key_comp`, `value_comp` — the comparison function in use

Non-member: lexicographic `operator==`/`<=>` (C++20 replaces the
individual comparisons with `<=>`), `std::swap`, `std::erase_if`
(C++20). A `pmr::map` alias and deduction guides (C++17) are also
provided.

### See also

- **unordered_map** — same key-value semantics, average O(1) instead of
  O(log n), no ordering (C++11)
- **multimap** — same, but allows duplicate keys

---
*Source: https://en.cppreference.com/w/cpp/container/map*
