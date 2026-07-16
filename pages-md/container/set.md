# std::set

A sorted collection of unique keys, kept in ascending order by a
comparison function (`std::less<Key>` by default). Typically
implemented as a red-black tree. Use it when you need a membership
test *and* ordered iteration or range queries; if you only need
membership, `std::unordered_set` is usually faster.

Search, insertion, and removal are all O(log n).

```cpp skip
std::set<int> s;
std::set<int> s{3, 1, 4, 1, 5};      // duplicates dropped

s.insert(x);                         // returns {iterator, bool inserted}
s.emplace(args...);                  // construct in place

s.find(x);                           // iterator, or end() if absent
s.contains(x);                       // bool                            (C++20)
s.count(x);                          // 0 or 1 (elements are unique)
s.erase(x);

s.lower_bound(x); s.upper_bound(x);  // range queries, O(log n)

for (int x : s) { ... }              // visits elements in ascending order
```

### Guarantees and costs

- `find`, `count`, `contains`, `insert`, `emplace`, `erase`,
  `lower_bound`, `upper_bound`: all O(log n).
- Iterators, pointers, and references to existing elements are never
  invalidated by `insert`, `emplace`, or `erase`, except for the
  element actually erased — nodes don't move.
- `erase(it)` returns an iterator to the element that followed the
  erased one.
- Elements are immutable through the container (`iterator` and
  `const_iterator` may even be the same type): modifying an element in
  place would break the ordering invariant, so only erase-and-reinsert
  changes a value.

### Gotchas

- Use the member `s.find(x)` / `s.count(x)`, not the
  `std::find(s.begin(), s.end(), x)` algorithm — the member is
  O(log n), the generic algorithm is a linear O(n) scan that ignores
  the ordering.
- Because `iterator` and `const_iterator` may alias the same type,
  overloading a function on both can silently violate the One
  Definition Rule; write one overload taking `const_iterator` (an
  `iterator` converts to it implicitly) instead.
- Need duplicate keys? Use `std::multiset`. Don't need ordering?
  `std::unordered_set` gives average O(1) instead of O(log n).

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::set<int> s{3, 1, 4, 1, 5};   // duplicates dropped

    auto [it, inserted] = s.insert(9);
    std::cout << "inserted 9: " << std::boolalpha << inserted << '\n';

    for (int x : s)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

```text
inserted 9: true
1 3 4 5 9 
```

### Reference

```cpp skip
template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class set;  // (1)
namespace pmr {
    template< class Key, class Compare = std::less<Key> >
    using set = std::set<Key, Compare, std::pmr::polymorphic_allocator<Key>>;
}  // (2) (since C++17)
```

Two keys `a`, `b` are equivalent (treated as the same key) when
neither compares less than the other: `!comp(a, b) && !comp(b, a)` —
this is the uniqueness test the container uses, not `operator==`.
`value_type` is `Key`; `iterator` is a constant
LegacyBidirectionalIterator. `set` meets the requirements of Container,
AllocatorAwareContainer, AssociativeContainer, and ReversibleContainer.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `get_allocator`

  Iterators
  - `begin`/`cbegin`, `end`/`cend` (C++11)
  - `rbegin`/`crbegin`, `rend`/`crend` (C++11)

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `emplace`, `emplace_hint` (C++11)
  - `erase`
  - `swap`
  - `extract`, `merge` (C++17) — move nodes out of / between containers
    without copying keys

  Lookup
  - `count`, `find`, `contains` (C++20), `equal_range`
  - `lower_bound`, `upper_bound`

  Observers
  - `key_comp`, `value_comp` — the comparison function in use

Non-member: lexicographic `operator==`/`<=>` (C++20 replaces the
individual comparisons with `<=>`), `std::swap`, `std::erase_if`
(C++20). A `pmr::set` alias and deduction guides (C++17) are also
provided.

### See also

- **unordered_set** — same uniqueness semantics, average O(1) instead
  of O(log n), no ordering (C++11)
- **multiset** — same, but allows duplicate keys

---
*Source: https://en.cppreference.com/w/cpp/container/set*
