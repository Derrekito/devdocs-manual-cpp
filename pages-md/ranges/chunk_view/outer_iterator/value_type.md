# std::ranges::chunk_view<V>::*outer-iterator*::value_type

```cpp
struct value_type : view_interface<value_type>  // (since C++23)
```

A value type of the iterator `chunk_view::outer-iterator`, formed when `V`
models `input_range`.

### Data members

- **`parent_` (private)** — A pointer to the "parent object" of type
  `ranges::chunk_view*` (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an `value_type` (public member
  function)
- **begin (C++23)** — returns the beginning `chunk_view::inner-iterator` (public
  member function)
- **end (C++23)** — returns the default sentinel (public member function)
- **size (C++23)** — returns the size of the view (public member function)

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

## std::ranges::chunk_view::*outer-iterator*::value_type::value_type

```cpp
private:
// exposition only
constexpr explicit value_type( chunk_view& parent );  // (since C++23)
```

Constructs the `value_type` object so that `parent_` is initialized with
`std::addressof(parent)`.

### Parameters

- **parent** — the `chunk_view` object

## std::ranges::chunk_view::*outer-iterator*::value_type::begin

```cpp
constexpr /*inner-iterator*/ begin() const noexcept;  // (since C++23)
```

Equivalent to `return /*inner-iterator*/(*parent_);`.

## std::ranges::chunk_view::*outer-iterator*::value_type::end

```cpp
constexpr std::default_sentinel_t end() const noexcept;  // (since C++23)
```

Equivalent to `return std::default_sentinel;`.

## std::ranges::chunk_view::*outer-iterator*::value_type::size

```cpp
constexpr auto size() const
    requires std::sized_sentinel_for<ranges::sentinel_t<V>, ranges::iterator_t<V>>;  // (since C++23)
```

Returns the size of the view.

Equivalent to
`return /*to-unsigned-like*/( ranges::min(parent_->remainder_,
ranges::end(parent_->base_) - *parent_->current_));`

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- ***outer_iterator* (C++23)** — the output ("chunk-wise") iterator type when
  `V` models `input_range` (exposition-only member class of
  `std::ranges::chunk_view<V>`*)
- ***inner_iterator* (C++23)** — the inner ("element-wise") iterator type when
  `V` models `input_range` (exposition-only member class of
  `std::ranges::chunk_view<V>`*)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/outer_iterator/value_type*
