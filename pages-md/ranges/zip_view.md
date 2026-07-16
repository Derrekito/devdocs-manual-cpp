# std::ranges::views::zip, std::ranges::zip_view

```cpp
template< ranges::input_range... Views >
    requires (ranges::view<Views> && ...) && (sizeof...(Views) > 0)
class zip_view
    : public ranges::view_interface<zip_view<Views...>>  // (1) (since C++23)
namespace views {
    inline constexpr /*unspecified*/ zip = /*unspecified*/;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range... Rs >
    requires /* see below */
constexpr auto zip( Rs&&... rs );  // (since C++23)
```

1) `zip_view` is a range adaptor that takes one or more `view`s, and produces a
   `view` whose `i`th element is a tuple-like value consisting of the `i`th
   elements of all views. The size of produced view is the minimum of sizes of
   all adapted views.

2) `views::zip` is a customization point object. When calling with no argument,
   `views::zip()` is expression-equivalent to
   `auto(views::empty<std::tuple<>>)`. Otherwise, `views::zip(rs...)` is
   *expression-equivalent* to
   `ranges::zip_view<views::all_t<decltype((rs))>...>(rs...)`.

`zip_view` always models `input_range`, and models `forward_range`,
`bidirectional_range`, `random_access_range`, or `sized_range` if all adapted
`view` types model the corresponding concept.

`zip_view` models `common_range` if

- `sizeof...(Views)` is equal to `1`, and the only adapted view type models
  `common_range`, or
- at least one adapted view type does not model `bidirectional_range`, and every
  adapted view type models `common_range`, or
- every adapted view type models both `random_access_range` and `sized_range`.

### Customization point objects

The name `views::zip` denotes a *customization point object*, which is a const
function object of a literal `semiregular` class type. For exposition purposes,
the cv-unqualified version of its type is denoted as `__zip_fn`.

All instances of `__zip_fn` are equal. The effects of invoking different
instances of type `__zip_fn` on the same arguments are equivalent, regardless of
whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `views::zip` can be copied freely and its copies can be
used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::zip` above, `__zip_fn` models

- `std::invocable<__zip_fn, Args...>`,
- `std::invocable<const __zip_fn, Args...>`,
- `std::invocable<__zip_fn&, Args...>`, and
- `std::invocable<const __zip_fn&, Args...>`.

Otherwise, no function call operator of `__zip_fn` participates in overload
resolution.

### Data members

- **`views_` (private)** — A tuple of type `std::tuple<Views...>` that holds all
  adapted view objects. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `zip_view` (public member function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++23)** — returns the number of elements. Provided only if each
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
- ***sentinel* (C++23)** — the sentinel type used when `zip_view` is not a
  `common_range` (exposition-only member class template*)

### Helper templates

```cpp
template< class... Views >
inline constexpr bool enable_borrowed_range<ranges::zip_view<Views...>> =
    (ranges::enable_borrowed_range<Views> && ...);  // (since C++23)
```

This specialization of `ranges::enable_borrowed_range` makes `zip_view` satisfy
`borrowed_range` when each underlying view satisfies it.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_zip` | 202110L | (C++23) | `std::ranges::zip_view`,
      `std::ranges::zip_transform_view`, `std::ranges::adjacent_view`,
      `std::ranges::adjacent_transform_view`

### Example

```cpp
#include <array>
#include <iostream>
#include <list>
#include <ranges>
#include <string>
#include <tuple>
#include <vector>

void print(auto const rem, auto const& range)
{
    for (std::cout << rem; auto const& elem : range)
        std::cout << elem << ' ';
    std::cout << '\n';
}

int main()
{
    auto x = std::vector{1, 2, 3, 4};
    auto y = std::list<std::string>{"α", "β", "γ", "δ", "ε"};
    auto z = std::array{'A', 'B', 'C', 'D', 'E', 'F'};

    print("Source views:", "");
    print("x: ", x);
    print("y: ", y);
    print("z: ", z);

    print("\nzip(x,y,z):", "");

    for (std::tuple<int&, std::string&, char&> elem : std::views::zip(x, y, z))
    {
        std::cout << std::get<0>(elem) << ' '
                  << std::get<1>(elem) << ' '
                  << std::get<2>(elem) << '\n';

        std::get<char&>(elem) += ('a' - 'A'); // modifies the element of z
    }

    print("\nAfter modification, z: ", z);
}
```

Output:

```text
Source views:
x: 1 2 3 4
y: α β γ δ ε
z: A B C D E F

zip(x,y,z):
1 α A
2 β B
3 γ C
4 δ D

After modification, z: a b c d E F
```

### See also

- **ranges::zip_transform_viewviews::zip_transform (C++23)** — a `view`
  consisting of tuples of results of application of a transformation function to
  corresponding elements of the adapted views (class template) (customization
  point object)
- **ranges::elements_viewviews::elements (C++20)** — takes a `view` consisting
  of `tuple-like` values and a number N and produces a `view` of Nth element of
  each tuple (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view*
