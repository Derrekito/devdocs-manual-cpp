# std::list

`std::list` is a doubly-linked list: inserting or erasing an element
anywhere takes O(1) once you hold an iterator to the position, and
every other iterator or reference into the list stays valid across
that change — only the iterator/reference to the erased element
itself is invalidated. What it gives up is random access: there is no
`operator[]`, and reaching the nth element means walking N links.
Reach for it when code needs to hold iterators across repeated
mid-sequence insertions and erasures; otherwise `std::vector` is the
simpler default.

```cpp skip
std::list<int> l;                    // empty
std::list<int> l{1, 2, 3};           // list of elements

l.push_back(x); l.push_front(x);     // O(1) at either end
l.insert(it, x);                     // O(1) given a valid iterator
l.erase(it);                         // O(1) given a valid iterator
l.splice(pos, other);                // move nodes from another list in place
l.sort(); l.merge(other);            // member sort/merge — no random access,
                                      // so std::sort doesn't apply here
l.remove(x); l.remove_if(pred);      // erase matching elements
l.unique();                          // drop consecutive duplicates
```

### Guarantees and costs

- Insertion and erasure anywhere: O(1), given an iterator to the
  position — no shifting of other elements.
- No random access: advancing to the nth element is O(n).
- `size()`: O(1).
- Iterator/reference invalidation: adding, removing, or moving
  elements — including splicing nodes in from another list — never
  invalidates other iterators or references. An iterator or reference
  is invalidated only when the element it refers to is itself erased.
- `T` must be Erasable (roughly: destructible); operations that copy
  or assign elements require `T` to be CopyConstructible /
  CopyAssignable respectively (pre-C++11, `T` had to be
  CopyConstructible unconditionally, and CopyAssignable was required
  even when `operator=`/`assign` went unused — both requirements were
  loosened by later defect reports).
- Since C++17, the container (but not its members) can hold an
  incomplete element type if the allocator satisfies the allocator
  completeness requirements.

### Gotchas

- There is no `operator[]`/`at` — indexed access requires manual
  iteration and is O(n) per lookup, which defeats the point of
  reaching for a container at all if you need frequent indexed access.
- `std::sort` needs random-access iterators and won't compile on a
  `list`; use the member `l.sort()` instead. The same applies to
  `merge`, `remove`/`remove_if`, `reverse`, and `unique` — these are
  member functions here, not the `<algorithm>` free functions, because
  they can exploit the linked structure directly.
- Comparison operators are lexicographic, like other sequence
  containers; C++20 replaced the individual `<`/`<=`/`>`/`>=`
  overloads with `<=>`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <list>

int main()
{
    std::list<int> l = {7, 5, 16, 8};

    l.push_front(25);
    l.push_back(13);

    auto it = std::find(l.begin(), l.end(), 16);
    if (it != l.end())
        l.insert(it, 42);

    std::cout << "l = { ";
    for (int n : l)
        std::cout << n << ", ";
    std::cout << "};\n";
}
```

```text
l = { 25, 7, 5, 42, 16, 8, 13, };
```

### Reference

```cpp skip
template<
    class T,
    class Allocator = std::allocator<T>
> class list;  // (1)
namespace pmr {
    template< class T >
    using list = std::list<T, std::pmr::polymorphic_allocator<T>>;
}  // (2) (since C++17)
```

`list` meets the requirements of Container, AllocatorAwareContainer,
SequenceContainer, and ReversibleContainer. `Allocator` must meet the
Allocator requirements; it is ill-formed (previously UB, before C++20)
for `Allocator::value_type` to differ from `T`.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `assign`,
  `assign_range` (C++23), `get_allocator`

  Element access
  - `front`, `back`

  Iterators
  - `begin`/`cbegin`, `end`/`cend` (C++11)
  - `rbegin`/`crbegin`, `rend`/`crend` (C++11)

  Capacity
  - `empty`, `size`, `max_size`

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `emplace` (C++11)
  - `erase`
  - `push_back`, `emplace_back` (C++11), `append_range` (C++23)
  - `pop_back`
  - `push_front`, `emplace_front` (C++11), `prepend_range` (C++23)
  - `pop_front`
  - `resize`
  - `swap`

  Operations
  - `merge` — merge two sorted lists
  - `splice` — move elements from another `list`
  - `remove`, `remove_if` — erase matching elements
  - `reverse`
  - `unique` — remove consecutive duplicates
  - `sort`

Non-member: lexicographic `operator==`/`<=>` (C++20 replaces the
individual comparison operators with `<=>`), `std::swap`,
`std::erase`/`std::erase_if` (C++20). A `pmr::list` alias and deduction
guides (C++17) are also provided.

### See also

- **forward_list** — singly-linked, more space-efficient, no `size()`
- **vector** — contiguous storage, random access, the usual default
- **deque** — double-ended queue, O(1) insert/erase at both ends

---
*Source: https://en.cppreference.com/w/cpp/container/list*
