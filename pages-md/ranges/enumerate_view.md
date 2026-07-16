# std::ranges::views::enumerate, std::ranges::enumerate_view

```cpp
template< ranges::view V >
    requires /*range-with-movable-references*/<V>
class enumerate_view
    : public ranges::view_interface<enumerate_view<V>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ enumerate = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr /* see below */ enumerate( R&& r );  // (since C++23)
Helper concepts
template< class R >
    concept /*range-with-movable-references*/ =
        ranges::input_range<R> &&
        std::move_constructible<ranges::range_reference_t<R>> &&
        std::move_constructible<ranges::range_rvalue_reference_t<R>>;  // (3) (exposition only*)
```

1) `enumerate_view` is a range adaptor that takes a `view` and produces a view
of `tuple`s. `i`th element (the tuple) of the resulting sequence holds:

- the value equal to `i`, which is a zero-based index of the element of
  underlying sequence, and
- the reference to the underlying element.

2) The name `views::enumerate` denotes a RangeAdaptorObject. Given a
   subexpression `e`, the expression `views::enumerate(e)` is
   expression-equivalent to `enumerate_view<views::all_t<decltype((e))>>(e)` for
   any suitable subexpression `e`.

3) Ensures that the reference type of the underlying type can be moved.

`enumerate_view` models the concepts `random_access_range`,
`bidirectional_range`, `forward_range`, `input_range`, `common_range`, and
`sized_range` when the underlying view `V` models respective concepts.

### Data members

- **`base_` (private)** — An iterator to the underlying sequence of type `V`.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `enumerate_view` (public member
  function)
- **base (C++23)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++23)** — returns the number of elements. Provided only if the
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

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++23)** — the sentinel type (exposition-only member class
  template*)

### Helper templates

```cpp
template< class View >
constexpr bool enable_borrowed_range<ranges::enumerate_view<View>> =
    std::ranges::enable_borrowed_range<View>;  // (since C++23)
```

This specialization of `std::ranges::enable_borrowed_range` makes
`enumerate_view` satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_enumerate` | 202302L | (C++23) |
      `std::ranges::enumerate_view`

### Example

```cpp
#include <iostream>
#include <map>
#include <ranges>
#include <vector>

int main()
{
    constexpr static auto v = {'A', 'B', 'C', 'D'};

    for (auto const [index, letter] : std::views::enumerate(v))
        std::cout << '(' << index << ':' << letter << ") ";
    std::cout << '\n';

#if __cpp_lib_ranges_to_container
    // create a map using the position of each element as key
    auto m = v | std::views::enumerate | std::ranges::to<std::map>();

    for (auto const [key, value] : m)
        std::cout << '[' << key << "]:" << value << ' ';
    std::cout << '\n';
#endif

    std::vector<int> numbers{1, 3, 5, 7};

    // num is mutable even with const, which does not propagate to reference
    // to make it const, use `std::views::enumerate(numbers) | std::views::as_const`
    // or `std::views::enumerate(std::as_const(numbers))`
    for (auto const [index, num] : std::views::enumerate(numbers))
    {
        ++num; // the type is int&
        std::cout << numbers[index] << ' ';
    }
    std::cout << '\n';
}
```

Possible output:

```text
(0:A) (1:B) (2:C) (3:D)
[0]:A [1]:B [2]:C [3]:D
2 4 6 8
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::iota_viewviews::iota (C++20)** — a `view` consisting of a sequence
  generated by repeatedly incrementing an initial value (class template)
  (customization point object)
- **ranges::zip_viewviews::zip (C++23)** — a `view` consisting of tuples of
  references to corresponding elements of the adapted views (class template)
  (customization point object)
- **ranges::elements_viewviews::elements (C++20)** — takes a `view` consisting
  of `tuple-like` values and a number N and produces a `view` of Nth element of
  each tuple (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view*
