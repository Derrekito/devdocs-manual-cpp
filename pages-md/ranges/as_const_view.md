# std::ranges::views::as_const, std::ranges::as_const_view

```cpp
template< ranges::view V >
    requires ranges::input_range<V>
class as_const_view
    : public ranges::view_interface<as_const_view<V>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ as_const = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr ranges::view auto as_const( R&& r );  // (since C++23)
```

1) A range adaptor that represents a view of underlying `view` that is also a
   `constant_range`. An `as_const_view` always has read-only elements (if not
   empty).
2) RangeAdaptorObject. Let `e` be a subexpression, let `T` be `decltype((e))`,
and let `U` be `std::remove_cvref_t<T>`. Then the expression
`views::as_const(e)` is expression-equivalent to:

- `views::all(e)`, if it is a well-formed expression and `views::all_t<T>`
  models `constant_range`;
- otherwise, `std::span<const X, Extent>(e)` for some type `X` and some extent
  `Extent` if `U` denotes `std::span<X, Extent>`;
- otherwise, `ranges::ref_view(static_cast<const X&>(e.base()))` if `U` denotes
  `ranges::ref_view<X>` for some type `X` and `const X` models `constant_range`;
- otherwise, `ranges::ref_view(static_cast<const U&>(e))` if `e` is an lvalue,
  `const U` models `constant_range`, and `U` does not model `view`.
- otherwise, `as_const_view{e}`.

`as_const_view` always models `constant_range`, and it models the
`contiguous_range`, `random_access_range`, `bidirectional_range`,
`forward_range`, `borrowed_range`, `common_range`, and `sized_range` when the
underlying view `V` models respective concepts.

### Data members

- **`base_` (private)** — The underlying view of type `V`. (exposition-only
  member object*)

### Member functions

- **(constructor)** — constructs an `as_const_view` (public member function)
- **base** — returns the underlying view `V` (public member function)
- **begin** — returns the beginning iterator of the `as_const_view` (public
  member function)
- **end** — returns the end iterator of the `as_const_view` (public member
  function)
- **size** — returns the size of the view if it is bounded (public member
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

## std::ranges::as_const_view::as_const_view

```cpp
as_const_view() requires std::default_initializable<V> = default;  // (1) (since C++23)
constexpr explicit as_const_view( V base );  // (2) (since C++23)
```

1) Value-initializes `base_` via its default member initializer (`= V()`).

2) Initializes `base_` with `std::move(base)`.

### Parameters

- **base** — a view

## std::ranges::as_const_view::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++23)
constexpr V base() &&;  // (2) (since C++23)
```

Returns the underlying view.

1) Copy-constructs the result from the underlying view. Equivalent to `return
   base_;`.

2) Move-constructs the result from the underlying view. Equivalent to `return
   std::move(base_);`.

## std::ranges::as_const_view::begin

```cpp
constexpr auto begin() requires (!__simple_view<V>);  // (1) (since C++23)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++23)
```

Returns the constant iterator of the view.

1,2) Equivalent to `return ranges::cbegin(base_);`

## std::ranges::as_const_view::end

```cpp
constexpr auto end() requires (!__simple_view<V>);  // (1) (since C++23)
constexpr auto end() const requires ranges::range<const V>;  // (2) (since C++23)
```

Returns the constant sentinel of the view.

1,2) Equivalent to `return ranges::cend(base_);`

## std::ranges::as_const_view::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++23)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++23)
```

Returns the size of the view if the view is bounded.

1,2) Equivalent to `return ranges::size(base_);`

### Deduction guides

```cpp
template< class R >
as_const_view( R&& ) -> as_const_view<views::all_t<R>>;  // (since C++23)
```

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<std::ranges::as_const_view<T>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++23)
```

This specialization of `std::ranges::enable_borrowed_range` makes
`as_const_view` satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_as_const` | 202207L | (C++23) |
      `std::ranges::as_const_view`, `std::const_iterator`

### Example

```cpp
#include <cassert>
#include <ranges>

int main()
{
    int x[]{1, 2, 3, 4, 5};

    auto v1 = x | std::views::drop(2);
    assert(v1.back() == 5);
    v1[0]++; // OK, can modify non-const element

    auto v2 = x | std::views::drop(2) | std::views::as_const;
    assert(v2.back() == 5);
    // v2[0]++; // Compile-time error, cannot modify read-only element
}
```

### See also

- **ranges::as_rvalue_viewviews::as_rvalue (C++23)** — a `view` of a sequence
  that casts each element to an rvalue (class template) (range adaptor object)
- **ranges::cbegin (C++20)** — returns an iterator to the beginning of a
  read-only range (customization point object)
- **ranges::cend (C++20)** — returns a sentinel indicating the end of a
  read-only range (customization point object)
- **as_const (C++17)** — obtains a reference to const to its argument (function
  template)
- **basic_const_iterator (C++23)** — iterator adaptor that converts an iterator
  into a constant iterator (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/as_const_view*
