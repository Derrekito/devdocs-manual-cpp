# std::ranges::split_view<V,Pattern>::*iterator*

```cpp
class /*iterator*/;  // (since C++20) (exposition only*)
```

The return type of `split_view::begin`. This is a `forward_iterator`, so it is
expected that `V` models at least `forward_range`.

### Member types

- **`iterator_concept`** — `std::forward_iterator_tag`
- **`iterator_category`** — `std::input_iterator_tag`
- **`value_type`** — `ranges::subrange<ranges::iterator_t<V>>`
- **`difference_type`** — `ranges::range_difference_t<V>`

### Data members

- **`parent_` (private)** — A pointer of type `ranges::split_view<V, Pattern>*`
  to the parent `split_view` object. (exposition-only member object*)
- **`cur_` (private)** — An iterator of type `ranges::iterator_t<V>` into the
  underlying `view` that points to the begin of a current subrange.
  (exposition-only member object*)
- **`next_` (private)** — A subrange of type
  `ranges::subrange<ranges::iterator_t<V>>` to the position of the pattern next
  to the current subrange. (exposition-only member object*)
- **`trailing_empty_` (private)** — A boolean flag that indicates whether an
  empty trailing subrange (if any) was reached. (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **base (C++20)** — returns the underlying iterator (public member function)
- **operator* (C++20)** — returns the current subrange (public member function)
- **operator++operator++(int) (C++20)** — advances the iterator (public member
  function)

## std::ranges::split_view::*iterator*::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++20)
constexpr /*iterator*/( split_view& parent, ranges::iterator_t<V> current,
                        ranges::subrange<ranges::iterator_t<V>> next );  // (2) (since C++20)
```

1) Value-initializes non-static data members with their default member
initializers, that is

- `ranges::split_view* parent_ = nullptr;`,
- `ranges::iterator_t<V> cur_ = ranges::iterator_t<V>();`,
- `ranges::subrange<ranges::iterator_t<V>> next_ =
  ranges::subrange<ranges::iterator_t<V>>();`, and
- `bool trailing_empty_ = false;`.

2) Initializes non-static data members:

- `ranges::split_view* parent_ = std::addressof(parent);`,
- `ranges::iterator_t<V> cur_ = std::move(current);`,
- `ranges::subrange<ranges::iterator_t<V>> next_ = std::move(next);`, and
- `bool trailing_empty_ = false;`.

## std::ranges::split_view::*iterator*::base

```cpp
constexpr const ranges::iterator_t<V> base() const;  // (since C++20)
```

Equivalent to `return cur_;`.

## std::ranges::split_view::*iterator*::operator*

```cpp
constexpr ranges::range_reference_t<V> operator*() const;  // (since C++20)
```

Equivalent to `return {cur_, next_.begin()};`.

## std::ranges::split_view::*iterator*::operator++

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++20)
constexpr void operator++( int );  // (2) (since C++20)
```

1) Equivalent to `cur_ = next_.begin(); if (cur_ != ranges::end(parent_->base_))
   { if (cur_ = next_.end(); cur_ == ranges::end(parent_->base_)) {
   trailing_empty_ = true; next_ = {cur_, cur_}; } else next_ =
   parent_->find_next(cur_); } else trailing_empty_ = false; return *this;`

2) Equivalent to `auto tmp = *this; ++*this; return tmp;`.

### Non-member functions

- **operator== (C++20)** — compares the underlying iterators (function)

## operator==(std::ranges::split_view::*iterator*, std::ranges::split_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y );  // (since C++20)
```

Equivalent to `return x.cur_ == y.cur_ and x.trailing_empty_ ==
y.trailing_empty_;`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::iterator` is an associated class of the arguments.

---
*Source: https://en.cppreference.com/w/cpp/ranges/split_view/iterator*
