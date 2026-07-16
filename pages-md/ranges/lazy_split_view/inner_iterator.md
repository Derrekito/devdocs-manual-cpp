# std::ranges::lazy_split_view<V, Pattern>::*inner_iterator*

```cpp
template< bool Const >
struct /*inner_iterator*/;  // (since C++20) (exposition only*)
```

The return type of `lazy_split_view::outer_iterator::value_type::begin()`.

`Const` matches the template argument of `outer_iterator`.

### Member types

- **`Base`** — `const V` if `Const` is true, otherwise `V` (exposition-only
  member type*)
- **`iterator_concept` (C++20)** — equivalent to
  `/*outer_iterator*/<Const>::iterator_concept`, that is
  `std::forward_iterator_tag` if `Base` models `forward_range`, or
  `std::input_iterator_tag` otherwise.
- **`iterator_category` (C++20)** — Not present if `Base` does not model
  `forward_range`. `std::forward_iterator_tag` if
  `std::iterator_traits<ranges::iterator_t<Base>>::iterator_category` models
  `std::derived_from<std::forward_iterator_tag>`.
  `std::iterator_traits<ranges::iterator_t<Base>>::iterator_category` otherwise.
- **`value_type` (C++20)** — `ranges::range_value_t<Base>`.
- **`difference_type` (C++20)** — `ranges::range_difference_t<Base>`.

### Data members

- **`i_` (private)** — An iterator of type `/*outer_iterator*/<Const>` into the
  underlying `view` of the parent `lazy_split_view`. (exposition-only member
  object*)
- **`incremented_` (private)** — A boolean flag that indicates whether the
  operator++ was invoked on this object at least once. (exposition-only member
  object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **base (C++20)** — returns the underlying iterator (public member function)
- **operator* (C++20)** — returns the current element (public member function)
- **operator++operator++(int) (C++20)** — advances the iterator (public member
  function)

### Member functions

## std::ranges::lazy_split_view::*inner_iterator*::*inner_iterator*

```cpp
/*inner_iterator*/() = default;  // (1) (since C++20)
constexpr explicit /*inner_iterator*/( /*outer_iterator*/<Const> i );  // (2) (since C++20)
```

1) Value initializes data member `i_` via its default member initializer (=
   `/*outer_iterator*/<Const>()`).

2) Initializes `i_` with `std::move(i)`.

The data member `incremented_` is initialized with its default member
initializer to false.

## std::ranges::lazy_split_view::*inner_iterator*::*base*

```cpp
constexpr const ranges::iterator_t<Base>& base() const & noexcept;  // (1) (since C++20)
constexpr ranges::iterator_t<Base> base() &&
    requires ranges::forward_range<V>;  // (2) (since C++20)
```

Returns a copy of the underlying iterator.

1) Copy constructs the result from the underlying iterator. Equivalent to
   `return i_./*cur*/();`.

2) Move constructs the result from the underlying iterator. Equivalent to
   `return std::move(i_./*cur*/());`.

## std::ranges::lazy_split_view::*inner_iterator*::operator*

```cpp
constexpr decltype(auto) operator*() const;  // (since C++20)
```

Returns the element the underlying iterator points to.

Equivalent to `return *i_./*cur*/();`.

## std::ranges::lazy_split_view::*inner_iterator*::operator++

```cpp
constexpr /*inner_iterator*/& operator++();  // (1) (since C++20)
constexpr decltype(auto) operator++(int);  // (2) (since C++20)
```

1) The function body is equivalent to `incremented_ = true; if constexpr
   (!ranges::forward_range<Base>) { if constexpr (Pattern::size() == 0) return
   *this; } ++i_./*cur*/(); return *this;`

2) Equivalent to `if constexpr (ranges::forward_range<Base>) { auto tmp = *this;
   ++*this; return tmp; } else ++*this; // no return statement`

### Non-member functions

- **operator== (C++20)** — compares the iterators or the iterator and
  `std::default_sentinel` (function)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function)

## operator==(std::ranges::split_view::*inner_iterator*)

```cpp
friend constexpr bool operator==( const /*inner_iterator*/& x,
                                  const /*inner_iterator*/& y )
      requires forward_range<Base>;  // (1) (since C++20)
friend constexpr bool operator==( const /*inner_iterator*/& x,
                                  std::default_sentinel_t );  // (2) (since C++20)
```

1) Equivalent to `return x.i_./*cur*/() == y.i_./*cur*/();`.

2) The function body is equivalent to auto [pcur, pend] =
   ranges::subrange{x.i_.parent_->pattern_}; auto end =
   ranges::end(x.i_.parent_->base_); if constexpr (/*tiny_range*/<Pattern>) {
   const auto& cur = x.i_./*cur*/(); if (cur == end) return true; if (pcur ==
   pend) return x.incremented_; return *cur == *pcur; } else { auto cur =
   x.i_./*cur*/(); if (cur == end) return true; if (pcur == pend) return
   x.incremented_; do { if (*cur != *pcur) return false; if (++pcur == pend)
   return true; } while (++cur != end); return false; }

The `!=` operator is synthesized from `operator==`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::inner_iterator` is an associated class of the
arguments.

## iter_move(std::ranges::split_view::*inner_iterator*)

```cpp
friend constexpr decltype(auto) iter_move( const /*inner_iterator*/& i )
    noexcept(noexcept(ranges::iter_move(i.i_./*cur*/())));  // (since C++20)
```

Equivalent to `return ranges::iter_move(i.i_./*cur*/());`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::inner_iterator` is an associated class of the
arguments.

## iter_swap(std::ranges::split_view::*inner_iterator*)

```cpp
friend constexpr void iter_swap( const /*inner_iterator*/& x,
                                 const /*inner_iterator*/& y )
    noexcept(noexcept(ranges::iter_swap(x.i_.current, y.i_.current)))
    requires std::indirectly_swappable<ranges::iterator_t<Base>>;  // (since C++20)
```

Equivalent to `ranges::iter_swap(x.i_./*cur*/(), y.i_./*cur*/())`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::inner_iterator` is an associated class of the
arguments.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3591 | C++20 | the `&&` overload of `base` might invalidate outer
      iterators | constraints added
  LWG 3593 | C++20 | the `const&` overload of `base` returns a reference but
      might not be noexcept | made noexcept

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view/inner_iterator*
