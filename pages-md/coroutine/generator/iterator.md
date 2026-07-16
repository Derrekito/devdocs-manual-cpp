# std::generator<Ref,V,Allocator>::*iterator*

```cpp
class /*iterator*/;  // (since C++23) (exposition only*)
```

The return type of `generator::begin`.

Models `indirectly_readable` and `input_iterator`.

### Member types

- **`value_type`** — `std::generator::value`
- **`difference_type`** — `std::ptrdiff_t`

### Data members

- **`coroutine_` (private)** — A coroutine handle of type
  `std::coroutine_handle<std::generator::promise_type>`. (exposition-only member
  object*)

### Member functions

- **(constructor)** — constructs an iterator (public member function)
- **operator=** — assigns another iterator (public member function)
- **operator*** — returns an underlying value (public member function)
- **operator++operator++(int)** — advances the iterator (public member function)

## std::generator::*iterator*::*iterator*

```cpp
/*iterator*/( /*iterator*/&& other ) noexcept;  // (since C++23)
```

Initializes `coroutine_` with `std::exchange(other.coroutine_, {});`.

## std::generator::*iterator*::operator=

```cpp
/*iterator*/& operator=( /*iterator*/&& other ) noexcept;  // (since C++23)
```

Equivalent to `coroutine_ = std::exchange(other.coroutine_, {});`.

Returns: `*this`.

## std::generator::*iterator*::operator*

```cpp
reference operator*() const
    noexcept( std::is_nothrow_copy_constructible_v<reference> );  // (since C++23)
```

1. Let `reference` be the `std::generator`'s underlying type.
1. Let for some generator object `x` its `coroutine_` be in the stack
   `*x.active_`.
1. Let `x.active_->top()` refer to a suspended coroutine with promise object
   `p`.

Equivalent to `return static_cast<reference>(*p.value_);`.

## std::generator::*iterator*::operator++

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++23)
constexpr void operator++( int );  // (2) (since C++23)
```

1) Let for some generator object `x` the `coroutine_` be in the stack
   `*x.active_`.

Equivalent to `x.active_->top().resume()`.

Returns: `*this`.

2) Equivalent to `++*this;`.

### Non-member functions

- **operator== (C++23)** — compares the underlying iterator with a sentinel
  (function)

## operator==(std::generator::*iterator*)

```cpp
friend bool operator==( const /*iterator*/& i, std::default_sentinel_t );  // (since C++23)
```

Equivalent to `return i.coroutine_.done();`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::generator::iterator`
is an associated class of the arguments.

### Example

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/iterator*
