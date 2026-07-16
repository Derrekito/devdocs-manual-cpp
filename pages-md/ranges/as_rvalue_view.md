# std::ranges::views::as_rvalue, std::ranges::as_rvalue_view

```cpp
template< ranges::view V >
    requires ranges::input_range<V>
class as_rvalue_view
    : public ranges::view_interface<as_rvalue_view<V>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ as_rvalue = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr ranges::view auto as_rvalue( R&& r );  // (since C++23)
```

1) A range adaptor that represents a view of underlying `view` whose elements
   are rvalues.
2) RangeAdaptorObject. Let `e` be a subexpression and let `T` be
`decltype((e))`. Then the expression `views::as_rvalue(e)` is
expression-equivalent to:

- `views::all(e)`, if it is a well-formed expression and
  `std::same_as<ranges::range_rvalue_reference_t<T>,
  ranges::range_reference_t<T>>` is `true`;
- `as_rvalue_view{e}` otherwise.

### Data members

- **`base_` (private)** — The underlying view of type `V`. (exposition-only
  member object*)

### Member functions

- **(constructor)** — constructs an `as_rvalue_view` (public member function)
- **base** — returns the underlying view `V` (public member function)
- **begin** — returns the beginning iterator of the `as_rvalue_view` (public
  member function)
- **end** — returns the end iterator of the `as_rvalue_view` (public member
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
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

## std::ranges::as_rvalue_view::as_rvalue_view

```cpp
as_rvalue_view() requires std::default_initializable<V> = default;  // (1) (since C++23)
constexpr explicit as_rvalue_view( V base );  // (2) (since C++23)
```

1) Value-initializes `base_` via its default member initializer (`= V()`).

2) Initializes `base_` with `std::move(base)`.

### Parameters

- **base** — a view

## std::ranges::as_rvalue_view::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++23)
constexpr V base() &&;  // (2) (since C++23)
```

Returns the underlying view.

1) Copy-constructs the result from the underlying view. Equivalent to `return
   base_;`.

2) Move-constructs the result from the underlying view. Equivalent to `return
   std::move(base_);`.

## std::ranges::as_rvalue_view::begin

```cpp
constexpr auto begin() requires (!__simple_view<V>);  // (1) (since C++23)
constexpr auto begin() const requires ranges::range<const V>;  // (2) (since C++23)
```

1,2) Returns `std::move_iterator(ranges::begin(base_))`.

## std::ranges::as_rvalue_view::end

```cpp
constexpr auto end() requires (!__simple_view<V>);  // (1) (since C++23)
constexpr auto end() const requires ranges::range<const V>;  // (2) (since C++23)
```

1) Returns `std::move_iterator(ranges::end(base_))` if `V` models
   `common_range`, otherwise `std::move_sentinel(ranges::end(base_))`.

2) Returns `std::move_iterator(ranges::end(base_))` if `const V` models
   `common_range`, otherwise `std::move_sentinel(ranges::end(base_))`.

## std::ranges::as_rvalue_view::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++23)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++23)
```

Returns the size of the view if the view is bounded.

1,2) Equivalent to `return ranges::size(base_);`

### Deduction guides

```cpp
template< class R >
as_rvalue_view( R&& ) -> as_rvalue_view<views::all_t<R>>;  // (since C++23)
```

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<std::ranges::as_rvalue_view<T>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++23)
```

This specialization of `std::ranges::enable_borrowed_range` makes
`as_rvalue_view` satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_as_rvalue` | 202207L | (C++23) |
      `std::ranges::as_rvalue_view`

### Example

### See also

- **move_iterator (C++11)** — iterator adaptor which dereferences to an rvalue
  reference (class template)
- **move_sentinel (C++20)** — sentinel adaptor for use with `std::move_iterator`
  (class template)
- **ranges::as_const_viewviews::as_const (C++23)** — converts a `view` into a
  `constant_range` (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/as_rvalue_view*
