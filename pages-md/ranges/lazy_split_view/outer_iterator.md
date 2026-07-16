# std::ranges::lazy_split_view<V, Pattern>::*outer_iterator*

```cpp
template< bool Const >
struct /* outer_iterator */;  // (since C++20) (exposition only*)
```

The return type of `lazy_split_view::begin`, and of `lazy_split_view::end` when
the underlying view is a `common_range` and `forward_range`.

If either `V` or `Pattern` is not a simple view (e.g. if
ranges::iterator_t<const V> is invalid or different from ranges::iterator_t<V>),
`Const` is `true` for iterators returned from the const overloads, and `false`
otherwise. If `V` is a simple view, `Const` is `true` if and only if `V` is a
`forward_range`.

### Member types

- **`Parent`** — const ranges::lazy_split_view if `Const` is `true`, otherwise
  ranges::lazy_split_view (exposition-only member type*)
- **`Base`** — const V if `Const` is `true`, otherwise `V` (exposition-only
  member type*)
- **`iterator_concept` (C++20)** — `std::forward_iterator_tag` if `Base` models
  `forward_range`, otherwise `std::input_iterator_tag`
- **`iterator_category` (C++20)** — `std::input_iterator_tag` if `Base` models
  `forward_range`. Not present otherwise.
- **`value_type` (C++20)** — ranges::lazy_iterator_view<V, Pattern> ::/*
  outer_iterator */<Const>::value_type
- **`difference_type` (C++20)** — ranges::range_difference_t<Base>.

### Data members

- **`parent_` (private)** — A pointer of type `Parent*` to the parent
  `lazy_split_view` object (exposition-only member object*)
- **`current_` (private)** — An iterator of type ranges::iterator_t<Base> into
  the underlying `view`; present only if `V` models `forward_range`
  (exposition-only member object*)
- **`trailing_empty_` (private)** — A boolean flag that indicates whether an
  empty trailing subrange (if any) was reached (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **operator* (C++20)** — returns the current subrange (public member function)
- **operator++operator++(int) (C++20)** — advances the iterator (public member
  function)
- ***cur* (C++20)** — returns conditionally a reference to the `current_` (if
  present) or to the `*parent_->current_` (exposition-only member function*)

### Member functions

## std::ranges::lazy_split_view::*outer_iterator* ﻿::*outer_iterator*

```cpp
/* outer_iterator */() = default;  // (1) (since C++20)
constexpr explicit /* outer_iterator */( Parent& parent )
  requires (!ranges::forward_range<Base>);  // (2) (since C++20)
constexpr /* outer_iterator */( Parent& parent,
                                ranges::iterator_t<Base> current )
  requires ranges::forward_range<Base>;  // (3) (since C++20)
constexpr /* outer_iterator */( /* outer_iterator */<!Const> i )
  requires Const && std::convertible_to<ranges::iterator_t<V>,
                                        ranges::iterator_t<Base>>;  // (4) (since C++20)
```

1) Value initializes the non-static data members with their default member
initializer, that is:

- `parent_ = nullptr;`,
- `current_ = iterator_t<Base>();` (present only if `V` models `forward_range`),

2) Initializes `parent_` with `std::addressof(parent)`.

3) Initializes `parent_` with `std::addressof(parent)` and `current_` with
   `std::move(current)`.

4) Initializes `parent_` with `i.parent_`, `current_` with
   `std::move(i.current_)`, and `trailing_empty_` with `t.trailing_empty_`.

The `trailing_empty_` is initialized with its default member initializer to
false.

## std::ranges::lazy_split_view::*outer_iterator* ﻿::operator*

```cpp
constexpr value_type operator*() const;  // (since C++20)
```

Equivalent to `return value_type{*this};`.

## std::ranges::lazy_split_view::*outer_iterator* ﻿::operator++

```cpp
constexpr /* outer_iterator */& operator++();  // (1) (since C++20)
constexpr decltype(auto) operator++(int);  // (2) (since C++20)
```

1) The function body is equivalent to const auto end =
   ranges::end(parent_->base_); if (/* cur */() == end) { trailing_empty_ =
   false; return *this; } const auto [pbegin, pend] =
   ranges::subrange{parent_->pattern_}; if (pbegin == pend) ++/* cur */(); else
   if constexpr (/* tiny_range */<Pattern>) { /* cur */() =
   ranges::find(std::move(/* cur */()), end, *pbegin); if (/* cur */() != end) {
   ++/* cur */(); if (/* cur */() == end) trailing_empty_ = true; } } else { do
   { auto [b, p] = ranges::mismatch(/* cur */(), end, pbegin, pend); if (p ==
   pend) { /* cur */() = b; if (/* cur */() == end) trailing_empty_ = true;
   break; // The pattern matched; skip it } } while (++/* cur */() != end); }
   return *this;

2) Equivalent to if constexpr (ranges::forward_range<Base>) { auto tmp = *this;
   ++*this; return tmp; } else { ++*this; // no return statement }

## std::ranges::lazy_split_view::*outer_iterator* ﻿::*cur* ﻿()

```cpp
constexpr auto& /* cur */() noexcept;  // (1) (since C++20) (exposition only*)
constexpr auto& /* cur */() const noexcept;  // (2) (since C++20) (exposition only*)
```

This convenience member function is referred to from `/* outer_iterator
*/::operator++()`, from the non-member `operator==(const /* outer_iterator */&,
std::default_sentinel_t)`, and from some member functions of the possible
implementation of `inner_iterator`.

1,2) Equivalent to if constexpr (ranges::forward_range<V>) return current_; else
   return *parent->current_;

### Non-member functions

- **operator== (C++20)** — compares the underlying iterators or the underlying
  iterator and `std::default_sentinel` (function)

## operator==(std::ranges::split_view::*outer_iterator*)

```cpp
friend constexpr bool operator==( const /* outer_iterator */& x,
                                  const /* outer_iterator */& y )
      requires forward_range<Base>;  // (1) (since C++20)
friend constexpr bool operator==( const /* outer_iterator */& x,
                                  std::default_sentinel_t );  // (2) (since C++20)
```

1) Equivalent to `return x.current_ == y.current_ and x.trailing_empty_ ==
   y.trailing_empty_;`.

2) Equivalent to `return x./* cur */() == ranges::end(x.parent_->base_) and
   !x.trailing_empty_;`.

The `!=` operator is synthesized from `operator==`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::split_view::outer_iterator` is an associated class of the
arguments.

### Nested classes

- **value_type (C++20)** — the value type of the `outer_iterator` (public member
  class)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3904 | C++20 | `trailing_empty_` was not initialized in constructor
      overload (4) | initialized

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view/outer_iterator*
