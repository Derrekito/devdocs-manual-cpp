# std::ranges::views::elements, std::ranges::elements_view

```cpp
template< ranges::input_range V, std::size_t N >
    requires ranges::view<V> &&
             has-tuple-element<ranges::range_value_t<V>, N> &&
             has-tuple-element<std::remove_reference_t<
                                   ranges::range_reference_t<V>>, N> &&
             returnable-element<ranges::range_reference_t<V>, N>
class elements_view : public ranges::view_interface<elements_view<V, N>>;  // (1) (since C++20)
namespace views {
    template< std::size_t N >
    inline constexpr /* unspecified */ elements = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr ranges::view auto elements<N>( R&& r );  // (since C++20)
Helper concepts
template< class T, std::size_t N >
concept has-tuple-element =
    requires(T t) {
        typename std::tuple_size<T>::type;
        requires N < std::tuple_size_v<T>;
        typename std::tuple_element_t<N, T>;
        { std::get<N>(t) } -> std::convertible_to<
                                  const std::tuple_element_t<N, T>&>;
    };  // (until C++23) (exposition only*)
template< class T, std::size_t N >
concept has-tuple-element =
    tuple-like<T> && N < std::tuple_size_v<T>  // (since C++23) (exposition only*)
template< class T, std::size_t N >
concept returnable-element =
     std::is_reference_v<T> || std::move_constructible<
                                       std::tuple_element_t<N, T>>;  // (4) (exposition only*)
```

1) Accepts a `view` of tuple-like values, and issues a view with a value type of
   the `N`-th element of the adapted view's value-type.

2) Every specialization of `views::elements` is a RangeAdaptorObject. The
   expression `views::elements<M>(e)` is expression-equivalent to
   `elements_view<views::all_t<decltype((e))>, M>{e}` for any suitable
   subexpression `e` and constant expression `M`.

3) Ensures that the elements of the underlying view are tuple-like values, see
   `tuple-like`(since C++23).

4) Ensures that dangling references cannot be returned.

`elements_view` models the concepts `random_access_range`,
`bidirectional_range`, `forward_range`, `input_range`, `common_range`, and
`sized_range` when the underlying view `V` models respective concepts.

### Member functions

- **(constructor) (C++20)** — constructs a `elements_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)
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
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Nested classes

- ***iterator* (C++20)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++20)** — the sentinel type (exposition-only member class
  template*)

### Helper templates

```cpp
template<class T, std::size_t N>
inline constexpr bool enable_borrowed_range<std::ranges::elements_view<T, N>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes
`elements_view` satisfy `borrowed_range` when the underlying view satisfies it.

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string>
#include <tuple>
#include <vector>

int main()
{
    const std::vector<std::tuple<int, char, std::string>> vt
    {
        {1, 'A', "α"},
        {2, 'B', "β"},
        {3, 'C', "γ"},
        {4, 'D', "δ"},
        {5, 'E', "ε"},
    };

    for (int const e : std::views::elements<0>(vt))
        std::cout << e << ' ';
    std::cout << '\n';

    for (char const e : vt | std::views::elements<1>)
        std::cout << e << ' ';
    std::cout << '\n';

    for (std::string const& e : std::views::elements<2>(vt))
        std::cout << e << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5
A B C D E
α β γ δ ε
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3494 | C++20 | `elements_view` was never a `borrowed_range` | it is a
      `borrowed_range` if its underlying view is
  LWG 3502 | C++20 | dangling reference could be obtained from `elements_view` |
      such usage is forbidden

### See also

- **ranges::keys_viewviews::keys (C++20)** — takes a `view` consisting of
  pair-like values and produces a `view` of the first elements of each pair
  (class template) (range adaptor object)
- **ranges::values_viewviews::values (C++20)** — takes a `view` consisting of
  pair-like values and produces a `view` of the second elements of each pair
  (class template) (range adaptor object)
- **ranges::zip_viewviews::zip (C++23)** — a `view` consisting of tuples of
  references to corresponding elements of the adapted views (class template)
  (customization point object)
- **ranges::zip_transform_viewviews::zip_transform (C++23)** — a `view`
  consisting of tuples of results of application of a transformation function to
  corresponding elements of the adapted views (class template) (customization
  point object)
- **slice** — BLAS-like slice of a valarray: starting index, length, stride
  (class)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view*
