# std::vector

A dynamically-sized array. Elements live in one contiguous block, so
`v[i]` is a single pointer offset and `v.data()` can be passed anywhere
a C array is expected. This is the default sequence container — reach
for something else only when you have a specific reason (frequent
front insertion → `std::deque`; frequent middle insertion → `std::list`;
fixed size known at compile time → `std::array`).

Growth is automatic: capacity roughly doubles when it runs out, so
`push_back` is amortized O(1), but any individual call that triggers a
reallocation copies (or moves) every existing element. `reserve()` up
front avoids that when the final size is known.

```cpp skip
std::vector<int> v;              // empty
std::vector<int> v(10);          // 10 value-initialized elements
std::vector<int> v{1, 2, 3};     // list of elements
std::vector<int> v(n, x);        // n copies of x

v.push_back(x);                  // append, amortized O(1)
v.emplace_back(args...);         // construct in place at the end (C++11)
v[i];                            // unchecked access
v.at(i);                         // bounds-checked access, throws
v.reserve(n);                    // grow capacity without adding elements
v.size(); v.empty();
```

`std::vector<bool>` is a bit-packed specialization with different
guarantees — see below.

### Guarantees and costs

- Random access (`v[i]`, `v.at(i)`): O(1).
- `push_back` / `emplace_back`: amortized O(1); a single call can be
  O(n) when it triggers reallocation.
- Insertion or erasure anywhere but the end: O(n) — every element after
  the point of insertion/erasure is shifted.
- `reserve(n)` / `shrink_to_fit()`: reallocate only if the requested
  capacity differs from the current one; otherwise no-ops.
- Iterator/reference/pointer invalidation:
  - Read-only operations, `operator[]`, `at`, `front`, `back`, `data`:
    never invalidate anything.
  - `push_back`, `emplace_back`, `insert`, `emplace`, `resize`,
    `reserve`, `shrink_to_fit`: invalidate **everything** if the call
    changes capacity; if it doesn't, only `end()` (and, for
    insert/emplace, iterators at or after the insertion point) are
    invalidated.
  - `erase`: invalidates the erased element(s) and everything after
    them, including `end()`.
  - `pop_back`: invalidates the erased element and `end()`.
  - `clear`, `operator=`, `assign`: invalidate everything, always.
  - `swap`, `std::swap`: invalidate only `end()` on both containers;
    other iterators keep pointing into their (now swapped) container.
- Since C++20, `vector`'s member functions are usable in `constexpr`
  evaluation, though a `vector` object itself generally can't survive
  as a `constexpr` value (its heap storage must be released within the
  same constant evaluation).

### Gotchas

- Any reallocating call invalidates *all* iterators, pointers, and
  references into the vector — holding one across a `push_back` in a
  loop is a classic use-after-free.
- `std::vector<bool>` does not store real `bool`s: `operator[]` returns
  a proxy object, not `bool&`. Code that needs `T&` semantics (e.g.
  taking the address of an element, or generic code expecting
  `container<bool>` to behave like other containers) breaks. Use
  `std::vector<char>` or `std::deque<bool>` instead.
- `size()` returns an unsigned `size_type`; mixing it with a signed
  loop counter in a comparison triggers `-Wsign-compare` and can
  underflow if the counter goes negative.
- Erasing by value inside a loop with plain `erase` is O(n^2) and
  invalidates iterators after the erase point — use the erase-remove
  idiom or `std::erase`/`std::erase_if` (C++20) instead; see the
  container's notes for the pattern.

### Example

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v = {8, 4, 5, 9};

    v.push_back(6);
    v.push_back(9);
    v[2] = -1;

    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

```text
8 4 -1 9 6 9
```

### Reference

```cpp skip
template<
    class T,
    class Allocator = std::allocator<T>
> class vector;  // (1)
namespace pmr {
    template< class T >
    using vector = std::vector<T, std::pmr::polymorphic_allocator<T>>;
}  // (2) (since C++17)
```

`T` must be Erasable from the vector (roughly: destructible), and many
member functions impose the stricter CopyInsertable, MoveInsertable, or
CopyAssignable/MoveAssignable requirements depending on what they do
(until C++11, `T` had to be CopyAssignable and CopyConstructible
outright). Since C++17, the container itself (not its members) can be
instantiated with an incomplete element type if the allocator satisfies
the allocator completeness requirements. `Allocator` must meet the
Allocator requirements; since C++20 it is ill-formed (previously UB)
for `Allocator::value_type` to differ from `T`.

`vector` (for `T` other than `bool`) meets the requirements of
Container, AllocatorAwareContainer (C++11), SequenceContainer,
ContiguousContainer (C++17), and ReversibleContainer.

**Member functions**, grouped as upstream groups them:

- (constructor), (destructor), `operator=`, `assign`,
  `assign_range` (C++23), `get_allocator`

  Element access
  - `at` — bounds-checked access, throws `std::out_of_range`
  - `operator[]` — unchecked access
  - `front`, `back` — first / last element
  - `data` — pointer to the contiguous underlying storage

  Iterators
  - `begin`/`cbegin`, `end`/`cend` (C++11)
  - `rbegin`/`crbegin`, `rend`/`crend` (C++11)

  Capacity
  - `empty`, `size`, `max_size`
  - `reserve` — grow capacity without adding elements
  - `capacity` — current allocated capacity
  - `shrink_to_fit` — request capacity be reduced to fit size

  Modifiers
  - `clear`
  - `insert`, `insert_range` (C++23)
  - `emplace` (C++11)
  - `erase`
  - `push_back`, `emplace_back` (C++11), `append_range` (C++23)
  - `pop_back`
  - `resize`
  - `swap`

Non-member: lexicographic `operator==`/`<=>` (C++20 replaces the
individual comparison operators with `<=>`), `std::swap`,
`std::erase`/`std::erase_if` (C++20). A `pmr::vector` alias and
deduction guides (C++17) are also provided. There is a `vector<bool>`
specialization optimized for space (bit-packed storage).

### See also

- **array** — fixed-size array, no dynamic allocation
- **deque** — double-ended queue, O(1) insert/erase at both ends
- **list** — doubly-linked list, O(1) insert/erase anywhere given an
  iterator, no random access
- **vector_bool** — the `bool` specialization's actual interface

---
*Source: https://en.cppreference.com/w/cpp/container/vector*
