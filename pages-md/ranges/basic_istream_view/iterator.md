# std::ranges::basic_istream_view::*iterator*

```cpp
struct /*iterator*/;  // (since C++20) (exposition only*)
```

The return type of `basic_istream_view::begin`.

`/*iterator*/` is an `input_iterator`, but does not satisfy LegacyInputIterator,
and thus does not work with pre-C++20 algorithms.

### Member types

- **`iterator_concept` (C++20)** — `std::input_iterator_tag`
- **`difference_type` (C++20)** — `std::ptrdiff_t`
- **`value_type` (C++20)** — `Val`

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **operator= (C++20)** — the copy assignment operator is deleted;
  `/*iterator*/` is move-only (public member function)
- **operator++ (C++20)** — advances the iterator (public member function)
- **operator* (C++20)** — returns the current element (public member function)

## std::ranges::basic_istream_view::*iterator*::*iterator*

```cpp
constexpr explicit /*iterator*/( basic_istream_view& parent );  // (1) (since C++20)
/*iterator*/( const /*iterator*/& ) = delete;  // (2) (since C++20)
/*iterator*/( /*iterator*/&& ) = default;  // (3) (since C++20)
```

1) Constructs an iterator from the parent `basic_istream_view`.

2) The copy constructor is deleted. The iterator is not copyable.

3) The move constructor is defaulted. The iterator is movable.

## std::ranges::basic_istream_view::*iterator*::operator=

```cpp
/*iterator*/& operator=( const /*iterator*/& ) = delete;  // (1) (since C++20)
/*iterator*/& operator=( /*iterator*/&& ) = default;  // (2) (since C++20)
```

1) The copy assignment operator is deleted. The iterator is not copyable.

2) The move assignment operator is defaulted. The iterator is movable.

## std::ranges::basic_istream_view::*iterator*::operator++

```cpp
/*iterator*/& operator++();  // (1) (since C++20)
void operator++(int);  // (2) (since C++20)
```

Reads a value from the underlying stream and stores it into the parent
`basic_istream_view`.

### Return value

1) `*this`

2) (none)

## std::ranges::basic_istream_view::*iterator*::operator*

```cpp
Val& operator*() const;  // (since C++20)
```

Returns a reference to the stored value.

### Non-member functions

- **operator== (C++20)** — compares with a `std::default_sentinel_t` (public
  member function)

## operator==(std::ranges::basic_istream_view::*iterator*, std::default_sentinel)

```cpp
friend bool operator==( const /*iterator*/& x, std::default_sentinel_t );  // (since C++20)
```

Compares an iterator with `std::default_sentinel_t`.

Returns true if `*this` does not have a parent `basic_istream_view`, or if an
error has occurred on the underlying stream.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::basic_istream_view::iterator` is an associated class of the
arguments.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2325R3 | C++20 | default constructor was provided as C++20 iterators must be
      `default_initializable` | removed along with the requirement

---
*Source: https://en.cppreference.com/w/cpp/ranges/basic_istream_view/iterator*
