# std::ranges::repeat_view<W, Bound>::*iterator*

```cpp
struct /*iterator*/;  // (since C++23) (exposition only*)
```

The return type of `repeat_view::begin`.

### Member types

- **`index-type`** — `std::ptrdiff_t`, if `Bound` is the same as
  `std::unreachable_sentinel_t`. Otherwise, `Bound`. (exposition-only member
  type*)
- **`iterator_concept`** — `std::random_access_iterator_tag`
- **`iterator_category`** — `std::random_access_iterator_tag`
- **`value_type`** — `W`
- **`difference_type`** — `/*index-type*/`, if
  `/*is-signed-like*/</*index-type*/>` is `true`,
  `/*iota-diff-t*/(/*index-type*/)` otherwise, where `/*iota-diff-t*/` has the
  same meaning as in `iota_view`.

### Data members

- **`value_` (private)** — A pointer of type `const W*` that holds the pointer
  to the value to repeat. (exposition-only member object*)
- **`current_` (private)** — An object of type `/*index-type*/` that holds the
  current position. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — returns the current subrange (public member function)
- **operator[] (C++23)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

## std::ranges::repeat_view::*iterator*::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
constexpr explicit /*iterator*/(
    const W* value, /*index-type*/ b = /*index-type*/() );  // (2) (since C++23) (exposition only*)
```

1) Value initializes the data members:

- `value_` with `nullptr_t` via its default member initializer;
- `index_` via its default member initializer (`= /*index-type*/()`).

2) Value initializes `value_` with `value` and `bound_` with `b`. If `Bound` is
   not `std::unreachable_sentinel_t` then `b` must be non-negative. This
   constructor is not a part of the public interface.

## std::ranges::repeat_view::*iterator*::operator*

```cpp
constexpr const W& operator*() const noexcept;  // (since C++23)
```

Equivalent to `return *value_;`.

## std::ranges::repeat_view::*iterator*::operator[]

```cpp
constexpr const W& operator[]( difference_type n ) const noexcept;  // (since C++23)
```

Equivalent to `return *(*this + n);`.

## std::ranges::repeat_view::*iterator*::operator++

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++(int);  // (2) (since C++23)
```

1) Equivalent to `++current_; return *this;`.

2) Equivalent to `auto tmp = *this; ++*this; return tmp;`.

## std::ranges::repeat_view::*iterator*::operator--

```cpp
constexpr /*iterator*/& operator--();  // (1) (since C++23)
constexpr /*iterator*/ operator--(int);  // (2) (since C++23)
```

1) Equivalent to `--current_; return *this;`. If `Bound` is not
   `std::unreachable_sentinel_t` then `bound_` must be positive.

2) Equivalent to `auto tmp = *this; --*this; return tmp;`.

## std::ranges::repeat_view::*iterator*::operator+=

```cpp
constexpr /*iterator*/& operator+=( difference_type n );  // (since C++23)
```

Equivalent to `current_ += n; return *this;`. If `Bound` is not
`std::unreachable_sentinel_t` then `(bound_ + n)` must be non-negative.

## std::ranges::repeat_view::*iterator*::operator-=

```cpp
constexpr /*iterator*/& operator-=( difference_type n );  // (since C++23)
```

Equivalent to `current_ -= n; return *this;`. If `Bound` is not
`std::unreachable_sentinel_t`, then `(bound_ - n)` must be non-negative.

### Non-member functions

- **operator==operator<=> (C++23)** — compares the underlying iterators
  (function)
- **operator+operator- (C++23)** — performs iterator arithmetic (function)

## operator==, <=>(std::ranges::repeat_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y );  // (1) (since C++23)
friend constexpr auto operator<=>( const /*iterator*/& x, const /*iterator*/& y );  // (2) (since C++23)
```

1) Equivalent to `x.current_ == y.current_`.

2) Equivalent to `x.current_ <=> y.current_`.

The `!=` operator is synthesized from `operator==`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

## operator+(std::ranges::repeat_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator+( /*iterator*/ i, difference_type n );  // (1) (since C++23)
friend constexpr /*iterator*/ operator+( difference_type n, /*iterator*/ i );  // (2) (since C++23)
```

Equivalent to `i += n; return i;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

## operator-(std::ranges::repeat_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator-( /*iterator*/ i, difference_type n );  // (1) (since C++23)
friend constexpr difference_type operator-( const /*iterator*/& x,
                                            const /*iterator*/& y );  // (2) (since C++23)
```

1) Equivalent to `i -= n; return i;`.

2) Equivalent to `return static_cast<difference_type>(x.current_) -
   static_cast<difference_type>(y.current_);`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

### Notes

`iterator` is always `random_access_iterator`.

---
*Source: https://en.cppreference.com/w/cpp/ranges/repeat_view/iterator*
