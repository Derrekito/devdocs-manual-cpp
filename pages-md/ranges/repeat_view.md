# std::ranges::views::repeat, std::ranges::repeat_view

```cpp
template< std::move_constructible W,
          std::semiregular Bound = std::unreachable_sentinel_t >
    requires (std::is_object_v<W> && std::same_as<W, std::remove_cv_t<W>> &&
             (/*is-integer-like*/<Bound> ||
              std::same_as<Bound, std::unreachable_sentinel_t>))
class repeat_view : public ranges::view_interface<repeat_view<W, Bound>>  // (1) (since C++23)
namespace views {
    inline constexpr /*unspecified*/ repeat = /*unspecified*/;
}  // (2) (since C++23)
Call signature
template< class W >
    requires /* see below */
constexpr /* see below */ repeat( W&& value );  // (since C++23)
template< class W, class Bound >
    requires /* see below */
constexpr /* see below */ repeat( W&& value, Bound&& bound );  // (since C++23)
```

1) A range factory that generates a sequence of elements by repeatedly producing
   the same value. Can be either bounded or unbounded (infinite).

2) `views::repeat(e)` and `views::repeat(e, f)` are expression-equivalent to
   (has the same effect as) `repeat_view(e)` and `repeat_view(e, f)`
   respectively for any suitable subexpressions `e` and `f`.

### Customization point objects

The name `views::repeat` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__repeat_fn`.

All instances of `__repeat_fn` are equal. The effects of invoking different
instances of type `__repeat_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `views::repeat` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::repeat` above, `__repeat_fn` models

- `std::invocable<__repeat_fn, Args...>`,
- `std::invocable<const __repeat_fn, Args...>`,
- `std::invocable<__repeat_fn&, Args...>`, and
- `std::invocable<const __repeat_fn&, Args...>`.

Otherwise, no function call operator of `__repeat_fn` participates in overload
resolution.

### Data members

- **`value_` (private)** — The beginning value of wrapped type
  `movable-box``<W>`. (exposition-only member object*)
- **`bound_` (private)** — The sentinel value of type `Bound`. (exposition-only
  member object*)

### Member functions

- **(constructor)** — creates a `repeat_view` (public member function)
- **begin** — obtains the beginning iterator of a `repeat_view` (public member
  function)
- **end** — obtains the sentinel denoting the end of a `repeat_view` (public
  member function)
- **size** — obtains the size of a `repeat_view` if it is sized (public member
  function)

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

## std::ranges::repeat_view::repeat_view

```cpp
repeat_view() requires std::default_initializable<W> = default;  // (1) (since C++23)
constexpr explicit repeat_view( const W& value, Bound bound = Bound() );  // (2) (since C++23)
constexpr explicit repeat_view( W&& value, Bound bound = Bound() );  // (3) (since C++23)
template < class... WArgs, class... BoundArgs >
    requires std::constructible_from<W, WArgs...>
          && std::constructible_from<Bound, BoundArgs...>
constexpr explicit repeat( std::piecewise_construct_t,
                           std::tuple<WArgs...> value_args,
                           std::tuple<BoundArgs...> bound_args = std::tuple<>{} );  // (4) (since C++23)
```

1) Value-initializes `value_` and `bound_` via their default member initializers
   (`= W()` and `= Bound()`).

2) Initializes `value_` with `value` and initializes `bound_` with `bound`. The
   behavior is undefined if `Bound` is not `std::unreachable_sentinel_t` and
   `bool(bound >= 0)` is `false`.

3) Initializes `value_` with `std::move(value)` and initializes `bound_` with
   `bound`. The behavior is undefined if `Bound` is not
   `std::unreachable_sentinel_t` and `bool(bound >= 0)` is `false`.

4) Initializes `value_` and `bound_` through piecewise construction.

### Parameters

- **value** — the value to be repeatedly produced
- **bound** — the bound

## std::ranges::repeat_view::begin

```cpp
constexpr /*iterator*/ begin() const;  // (since C++23)
```

Returns an iterator initialized with `std::addressof(*value_)`.

## std::ranges::repeat_view::end

```cpp
constexpr /*iterator*/ end() const
    requires (!std::same_as<Bound, std::unreachable_sentinel_t>);  // (1) (since C++23)
constexpr std::unreachable_sentinel_t end() const;  // (2) (since C++23)
```

1) Returns an iterator initialized with `std::addressof(*value_)` and `bound_`.

2) Returns an `std::unreachable_sentinel`.

## std::ranges::repeat_view::size

```cpp
constexpr auto size() const
    requires (!std::same_as<Bound, std::unreachable_sentinel_t>);  // (since C++23)
```

Returns the size of the view if the view is bounded. Equivalent to `return
/*to-unsigned-like*/(bound_);`.

The exposition-only function template `to-unsigned-like` converts its argument
(which must be integer-like) to the corresponding unsigned version of the
argument type.

### Deduction guides

```cpp
template< class W, class Bound >
repeat_view( W, Bound ) -> repeat_view<W, Bound>;  // (since C++23)
```

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class*)

### Notes

If `Bound` is not `std::unreachable_sentinel_t`, the `repeat_view` models
`sized_range` and `common_range`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_repeat` | 202207L | (C++23) | `std::ranges::repeat_view`

### Example

```cpp
#include <iostream>
#include <ranges>
#include <string_view>
using namespace std::literals;

int main()
{
    // bounded overload
    for (auto s : std::views::repeat("C++"sv, 3))
        std::cout << s << ' ';
    std::cout << '\n';

    // unbounded overload
    for (auto s : std::views::repeat("I know that you know that"sv)
                | std::views::take(3))
        std::cout << s << ' ';
    std::cout << "...\n";
}
```

Output:

```text
C++ C++ C++
I know that you know that I know that you know that I know that you know that ...
```

### See also

- **ranges::iota_viewviews::iota (C++20)** — a `view` consisting of a sequence
  generated by repeatedly incrementing an initial value (class template)
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/repeat_view*
