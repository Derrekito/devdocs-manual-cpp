# std::ranges::views::common, std::ranges::common_view

```cpp
template< ranges::view V >
    requires (not ranges::common_range<V> and
              std::copyable<ranges::iterator_t<V>>)
class common_view
    : public ranges::view_interface<common_view<V>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ common = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr ranges::view auto common( R&& r );  // (since C++20)
```

1) Adapts a given `view` with different types for iterator/sentinel pair into a
   `view` that is also a `common_range`. A `common_view` always has the same
   iterator/sentinel type.
2) RangeAdaptorObject. Let `e` be a subexpression. Then the expression
`views::common(e)` is expression-equivalent to:

- `views::all(e)`, if it is a well-formed expression and `decltype((e))` models
  `common_range`;
- `common_view{e}` otherwise.

### Data members

- **`base_` (private)** — The underlying view of type `V`. (exposition-only
  member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `common_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator to the end (public member function)
- **size (C++20)** — returns the number of elements. Provided only if the
  underlying (adapted) range satisfies `sized_range`. (public member function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **data (C++20)** — gets the address of derived view's data. Provided if its
  iterator type satisfies `contiguous_iterator`. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<std::ranges::common_view<T>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `common_view`
satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

`common_view` can be useful for working with legacy algorithms that expect the
iterator and sentinel are of the same type.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <list>
#include <numeric>
#include <ranges>

int main()
{
    auto v1 = {1, 2, 3, 4, 5};
    auto i1 = std::counted_iterator{v1.begin(), std::ssize(v1)};
    auto r1 = std::ranges::subrange{i1, std::default_sentinel};
//  auto e1 = std::accumulate(r1.begin(), r1.end(), 0); // error: "common range" required
    auto c1 = std::ranges::common_view{r1};
    std::cout << "accumulate: " << std::accumulate(c1.begin(), c1.end(), 0) << '\n';

    // inherited from ranges::view_interface:
    std::cout << "c1.front(): " << c1.front() << '\n';
    std::cout << "c1.back(): " << c1.back() << '\n';
    std::cout << "c1.data(): " << c1.data() << '\n';
    std::cout << "c1[0]: " << c1[0] << '\n';

    auto v2 = std::list{1, 2, 3, 4, 5};
    auto i2 = std::counted_iterator{v2.begin(), std::ssize(v2)};
    auto r2 = std::ranges::subrange{i2, std::default_sentinel};
//  auto e2 = std::accumulate(r2.begin(), r2.end(), 0); // error: "common range" required
    auto c2 = std::ranges::common_view{ r2 };
    std::cout << "accumulate: " << std::accumulate(c2.begin(), c2.end(), 0) << '\n';

    // inherited from ranges::view_interface:
    std::cout << "c2.front(): " << c2.front() << '\n';
//  auto e3 = c2.back(); // error: "bidirectional range" required
//  auto e4 = c2.data(); // error: "contiguous range" required
//  auto e5 = c2[0];     // error: "random access range" required
}
```

Possible output:

```text
accumulate: 15
c1.front(): 1
c1.back(): 5
c1.data(): 0x7f19937f00d0
c1[0]: 1
accumulate: 15
c2.front(): 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3494 | C++20 | `common_view` was never a `borrowed_range` | it is a
      `borrowed_range` if its underlying view is

### See also

- **ranges::common_range (C++20)** — specifies that a range has identical
  iterator and sentinel types (concept)
- **common_iterator (C++20)** — adapts an iterator type and its sentinel into a
  common iterator type (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/common_view*
