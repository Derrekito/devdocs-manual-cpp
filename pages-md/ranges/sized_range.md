# std::ranges::sized_range, std::ranges::disable_sized_range

```cpp
template< class T >
concept sized_range = ranges::range<T> &&
    requires(T& t) {
        ranges::size(t);
    };  // (1) (since C++20)
template< class >
inline constexpr bool disable_sized_range = false;  // (2) (since C++20)
```

1) The `sized_range` concept specifies the requirements of a `range` type that
   knows its size in constant time with the `size` function.

2) `disable_sized_range` exists to allow use of range types that provide a
   `size` function (either as a member or as a non-member) but do not in fact
   model `sized_range`. Users may specialize `disable_sized_range` for
   cv-unqualified program-defined types. Such specializations shall be usable in
   constant expressions and have type `const bool`.

### Semantic requirements

1) Given an lvalue `t` of type `std::remove_reference_t<T>`, `T` models
`sized_range` only if

- `ranges::size(t)`
- if `ranges::iterator_t<T>` models `forward_iterator`, `ranges::size(t)` is
  well-defined regardless of the evaluation of `ranges::begin(t)` (in other
  words, a single-pass sized range may support a call to size only before the
  first call to begin, but a forward range must support size at all times).

### Notes

`disable_sized_range` cannot be used to opt-out a range whose iterator and
sentinel satisfy `sized_sentinel_for`; `std::disable_sized_sentinel_for` must be
used instead.

`disable_sized_range` cannot be specialized for array types or reference types.

### Example

```cpp
#include <forward_list>
#include <list>
#include <ranges>

static_assert
(
    std::ranges::sized_range<std::list<int>> and
    not std::ranges::sized_range<std::forward_list<int>>
);

int main() {}
```

### See also

- **ranges::random_access_range (C++20)** — specifies a range whose iterator
  type satisfies `random_access_iterator` (concept)
- **ranges::contiguous_range (C++20)** — specifies a range whose iterator type
  satisfies `contiguous_iterator` (concept)

---
*Source: https://en.cppreference.com/w/cpp/ranges/sized_range*
