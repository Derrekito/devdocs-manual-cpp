# std::forward_list

`std::forward_list` (C++11) is a singly-linked list: insertion and
removal anywhere is fast, but only *after* a given position — because
each node points forward only, there is no way to reach the previous
node without walking from the beginning. That is why its modifiers are
named `insert_after`/`erase_after` rather than `insert`/`erase`, and
why there is a `before_begin()` iterator to let you insert at the
front. It has no random access and, unlike `std::list`, no `size()`
member and no `back()`/`push_back()` — it trades those away for the
smallest possible per-node overhead. Compared to `std::list`, reach
for it when backward iteration isn't needed and the more
space-efficient of the two is worth the smaller interface.

```cpp skip
std::forward_list<int> l;                  // empty
std::forward_list<int> l{1, 2, 3};         // list of elements

l.push_front(x);                           // insert at the front
l.insert_after(it, x);                     // insert after a position
l.erase_after(it);                         // erase the element after a position
l.before_begin();                          // usable as the "position" for the front
l.sort(); l.merge(other);                  // member sort/merge
l.remove(x); l.remove_if(pred);
l.unique();
```

### Guarantees and costs

- Insertion and removal after a given position: O(1); there is no
  fast way to insert or erase *at* a position without an iterator to
  the one before it, since the list can only be walked forward.
- No random access, and no `size()` — computing the count means
  walking the whole list, so the standard deliberately leaves `size()`
  out rather than make it look like an O(1) operation.
- `forward_list` meets the requirements of Container (except for the
  `size` member and that `operator==`'s complexity is always linear),
  AllocatorAwareContainer, and SequenceContainer.
- Iterator/reference invalidation: adding, removing, or moving
  elements — within the list or across lists — never invalidates
  iterators or references to other elements. An iterator or reference
  is invalidated only when the element it refers to is removed via
  `erase_after`.
- `T` requires Erasable generally (a complete type meeting Erasable,
  with individual operations imposing stricter requirements); since
  C++17 the container itself (not its members) can hold an incomplete
  element type if the allocator satisfies the allocator completeness
  requirements.

### Gotchas

- Every modifier that touches an arbitrary position takes the
  iterator *before* that position (`insert_after`, `erase_after`), not
  the position itself — passing `it` when you mean "the position
  before `it`" is a common off-by-one.
- No `size()`: if you need a count, you must count manually (O(n)) or
  track it yourself. This is deliberate, not an oversight.
- No `back()`, `push_back()`, or `pop_back()` — only the front is
  cheap to reach; appending to the end requires walking the whole list
  first.

### Example

```cpp
#include <forward_list>
#include <iostream>

int main()
{
    std::forward_list<int> l = {7, 5, 16, 8};

    l.push_front(25);

    auto it = l.before_begin();
    for (auto cur = l.begin(); cur != l.end() && *cur != 16; ++cur)
        ++it;
    l.insert_after(it, 42);

    std::cout << "l = { ";
    for (int n : l)
        std::cout << n << ", ";
    std::cout << "};\n";
}
```

```text
l = { 25, 7, 5, 42, 16, 8, };
```

### Reference

```cpp skip
template<
    class T,
    class Allocator = std::allocator<T>
> class forward_list;  // (1) (since C++11)
namespace pmr {
    template< class T >
    using forward_list = std::forward_list<T, std::pmr::polymorphic_allocator<T>>;
}  // (2) (since C++17)
```

`Allocator` must meet the Allocator requirements; it is ill-formed
(previously UB, before C++20) for `Allocator::value_type` to differ
from `T`. `iterator`/`const_iterator` are LegacyForwardIterator, unlike
`list`'s bidirectional iterators.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `assign`,
  `assign_range` (C++23), `get_allocator`

  Element access
  - `front`

  Iterators
  - `before_begin`/`cbefore_begin` — iterator to the element before the
    beginning, for inserting at the front
  - `begin`/`cbegin`, `end`/`cend`

  Capacity
  - `empty`, `max_size` (no `size`)

  Modifiers
  - `clear`
  - `insert_after`, `insert_range_after` (C++23)
  - `emplace_after`
  - `erase_after`
  - `push_front`, `emplace_front`, `prepend_range` (C++23)
  - `pop_front`
  - `resize`
  - `swap`

  Operations
  - `merge` — merge two sorted lists
  - `splice_after` — move elements from another `forward_list`
  - `remove`, `remove_if`
  - `reverse`
  - `unique`
  - `sort`

Non-member: lexicographic `operator==`/`<=>` (C++11; C++20 replaces
the individual comparison operators with `<=>`, and `operator==`'s
complexity is always linear), `std::swap` (C++11),
`std::erase`/`std::erase_if` (C++20). A `pmr::forward_list` alias and
deduction guides (C++17) are also provided.

### See also

- **list** — doubly-linked, bidirectional iteration, has `size()`
- **vector** — contiguous storage, random access, the usual default

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list*
