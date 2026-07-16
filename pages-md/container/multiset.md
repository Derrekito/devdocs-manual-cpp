# std::multiset

`std::multiset` is `std::set` with one difference: it allows multiple
elements that compare equivalent to coexist. Elements stay sorted by
`Compare`; search, insertion, and removal are all O(log n). Since
elements double as keys, there is no `operator[]` here either — read
elements with `find`/`count`, or `equal_range` to get every equivalent
element at once.

```cpp skip
std::multiset<int> s;
std::multiset<int> s{1, 1, 2, 3};

s.insert(1);                       // always inserts, never overwrites
s.emplace(4);                      // construct in place            (C++11)

s.count(1);                        // number of equivalent elements
s.find(1);                         // iterator to *one* matching element
auto [lo, hi] = s.equal_range(1);  // iterator range over *all* of them

s.erase(1);                        // erases every equivalent element
```

### Guarantees and costs

- `find`, `count`, `insert`, `emplace`, `erase`, `equal_range`,
  `lower_bound`, `upper_bound`: O(log n).
- Elements that compare equivalent keep their relative insertion order
  (since C++11).
- `multiset` meets the requirements of Container,
  AllocatorAwareContainer, AssociativeContainer, and
  ReversibleContainer.
- Equivalence, not `operator==`, decides duplicates: two elements `a`
  and `b` are equivalent if `!comp(a, b) && !comp(b, a)`.
- `iterator` is a constant iterator — elements can't be modified
  through it, since mutating an element could break the container's
  ordering invariant. (`iterator` and `const_iterator` may be the same
  type; write overloads in terms of `const_iterator` to avoid an ODR
  violation.)
- `extract`/`merge` (C++17) move nodes out of or between containers
  without copying elements.

### Gotchas

- No `operator[]` — there is no single value to return for a key that
  may appear more than once.
- `find` returns *one* matching iterator, not all of them; iterate
  `equal_range` when duplicates matter.
- `erase(value)` removes every equivalent element at once; pass an
  iterator instead to remove just one of several duplicates.
- `Compare` must impose a strict weak ordering; elements that don't
  compare less either way are equivalent even if not `==`.

### Example

```cpp
#include <iostream>
#include <set>

int main()
{
    std::multiset<int> s{3, 1, 2, 1};

    auto [lo, hi] = s.equal_range(1);
    for (auto it = lo; it != hi; ++it)
    {
        if (it != lo)
            std::cout << ' ';
        std::cout << *it;
    }
    std::cout << '\n';

    std::cout << "count(1) = " << s.count(1) << '\n';
}
```

```text
1 1
count(1) = 2
```

### Reference

```cpp skip
template<
    class Key,
    class Compare = std::less<Key>,
    class Allocator = std::allocator<Key>
> class multiset;  // (1)
namespace pmr {
    template<
        class Key,
        class Compare = std::less<Key>
    > using multiset = std::multiset<Key, Compare, std::pmr::polymorphic_allocator<Key>>;
}  // (2) (since C++17)
```

`Key` must be CopyConstructible (a defect report closed a gap in the
original wording that omitted this). `value_type` is `Key`;
`node_type` (C++17) is a specialization of the node handle type shared
with `set`.

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
`std::erase_if` (C++20; unlike sequence containers, `multiset` has no
free-function `erase`). A `pmr::multiset` alias and deduction guides
(C++17) are also provided.

### See also

- **set** — same sorted-element container, but unique elements
- **multimap** — same duplicate-key semantics, with a mapped value
- **unordered_multiset** — same duplicate-key semantics, hashed
  instead of sorted

---
*Source: https://en.cppreference.com/w/cpp/container/multiset*
