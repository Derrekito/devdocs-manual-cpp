# std::ranges::views::cartesian_product, std::ranges::cartesian_product_view

```cpp
template< ranges::input_range First, ranges::forward_range... Vs >
    requires (ranges::view<First> && ... && ranges::view<Vs>)
class cartesian_product_view
    : public ranges::view_interface<cartesian_product_view<First, Vs...>>  // (1) (since C++23)
namespace views {
    inline constexpr /*unspecified*/ cartesian_product = /*unspecified*/;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range... Rs >
    requires /* see below */
constexpr auto cartesian_product( Rs&&... rs );  // (since C++23)
Helper concepts
template< bool Const, class First, class... Vs >
concept __cartesian_product_is_random_access =
    (ranges::random_access_range<__maybe_const<Const, First>> && ... &&
        (ranges::random_access_range<__maybe_const<Const, Vs>> &&
            ranges::sized_range<__maybe_const<Const, Vs>>));  // (3) (exposition only*)
template< class R >
concept __cartesian_product_common_arg =
    ranges::common_range<R> ||
        (ranges::sized_range<R> && ranges::random_access_range<R>);  // (4) (exposition only*)
template< bool Const, class First, class... Vs >
concept __cartesian_product_is_bidirectional =
    (ranges::bidirectional_range<__maybe_const<Const, First>> && ... &&
        (ranges::bidirectional_range<__maybe_const<Const, Vs>> &&
            __cartesian_product_common_arg<__maybe_const<Const, Vs>>));  // (5) (exposition only*)
template< class First, class... Vs >
concept __cartesian_product_is_common =
    __cartesian_product_common_arg<First>;  // (6) (exposition only*)
template< class... Vs >
concept __cartesian_product_is_sized =
    (ranges::sized_range<Vs> && ...);  // (7) (exposition only*)
template< bool Const, template<class> class FirstSent, class First, class... Vs >
concept __cartesian_is_sized_sentinel =
    (ranges::sized_sentinel_for<FirstSent<__maybe_const<Const, First>>,
        ranges::iterator_t<__maybe_const<Const, First>>> && ... &&
            (ranges::sized_range<__maybe_const<Const, Vs>> &&
                ranges::sized_sentinel_for<iterator_t<__maybe_const<Const, Vs>>,
                    ranges::iterator_t<__maybe_const<Const, Vs>>>));  // (8) (exposition only*)
Helper function templates
template< __cartesian_product_common_arg R >
constexpr auto __cartesian_common_arg_end( R& r ) {
    if constexpr (ranges::common_range<R>)
        return ranges::end(r);
    else
        return ranges::begin(r) + ranges::distance(r);
}  // (9) (exposition only*)
```

1) `cartesian_product_view` is a range adaptor that takes *n* `view`s, where *n
   > 0*, and produces a `view` of tuples calculated by the n-ary cartesian
   product of the provided ranges. The size of produced view is a multiple of
   sizes of provided ranges, while each element is a tuple (of references) of
   the size *n*.
2) `views::cartesian_product` is a customization point object.

- When calling with no argument, `views::cartesian_product()` is
  expression-equivalent to `views::single(std::tuple())`.
- Otherwise, `views::cartesian_product(rs...)` is expression-equivalent to
  `ranges::cartesian_product_view<views::all_t<decltype((rs))>...>(rs...)`.

3) Determines if `cartesian_product` is a random access range (see also
   `random_access_range`).

4) Determines if `cartesian_product` is a common range (see also
   `common_range`).

5) Determines if `cartesian_product` is a bidirectional range (see also
   `bidirectional_range`).

6) Determines if `cartesian_product` satisfies the helper concept
   `__cartesian_product_is_common` (see also `common_range`).

7) Determines if `cartesian_product` is a sized range (see also `sized_range`).

8) Determines if `cartesian_product` uses sized sentinel.

9) Returns the end of the produced `view`. Participates in overload resolution
   only if `cartesian_product` satisfies the helper concept
   `__cartesian_product_common_arg`.

The `First` `range` passed to `cartesian_product_view` is treated specially,
since it is only passed through a single time. As a result, several constrains
are relaxed on it:

- `First` is an `input_range` instead of `forward_range`;
- `First` does not have to be a `sized_range` in order for the
  `cartesian_product_view` to be `random_access_range` or `common_range`;
- `First` does not have to be `common_range` in order for the
  `cartesian_product_view` to be `bidirectional_range`.

### Customization point objects

The name `views::cartesian_product` denotes a *customization point object*,
which is a const function object of a literal `semiregular` class type. For
exposition purposes, the cv-unqualified version of its type is denoted as
`__cartesian_product_fn`.

All instances of `__cartesian_product_fn` are equal. The effects of invoking
different instances of type `__cartesian_product_fn` on the same arguments are
equivalent, regardless of whether the expression denoting the instance is an
lvalue or rvalue, and is const-qualified or not (however, a volatile-qualified
instance is not required to be invocable). Thus, `views::cartesian_product` can
be copied freely and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::cartesian_product` above,
`__cartesian_product_fn` models

- `std::invocable<__cartesian_product_fn, Args...>`,
- `std::invocable<const __cartesian_product_fn, Args...>`,
- `std::invocable<__cartesian_product_fn&, Args...>`, and
- `std::invocable<const __cartesian_product_fn&, Args...>`.

Otherwise, no function call operator of `__cartesian_product_fn` participates in
overload resolution.

### Data members

- **`base_` (private)** — An object of type `std::tuple<First, Vs...>` that
  holds all adapted `view` objects. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `cartesian_product_view` (public
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

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_cartesian_product` | 202207L | (C++23) |
      `std::ranges::cartesian_product_view`

### Example

```cpp
#include <array>
#include <iostream>
#include <list>
#include <ranges>
#include <string>
#include <vector>

void print(std::tuple<char const&, int const&, std::string const&> t, int pos)
{
    const auto& [a, b, c] = t;
    std::cout << '(' << a << ' ' << b << ' ' << c << ')' << (pos % 4 ? " " : "\n");
}

int main()
{
    const auto x = std::array{'A', 'B'};
    const auto y = std::vector{1, 2, 3};
    const auto z = std::list<std::string>{"α", "β", "γ", "δ"};

    for (int i{1}; auto const& tuple : std::views::cartesian_product(x, y, z))
        print(tuple, i++);
}
```

Output:

```text
(A 1 α) (A 1 β) (A 1 γ) (A 1 δ)
(A 2 α) (A 2 β) (A 2 γ) (A 2 δ)
(A 3 α) (A 3 β) (A 3 γ) (A 3 δ)
(B 1 α) (B 1 β) (B 1 γ) (B 1 δ)
(B 2 α) (B 2 β) (B 2 γ) (B 2 δ)
(B 3 α) (B 3 β) (B 3 γ) (B 3 δ)
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::zip_viewviews::zip (C++23)** — a `view` consisting of tuples of
  references to corresponding elements of the adapted views (class template)
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view*
