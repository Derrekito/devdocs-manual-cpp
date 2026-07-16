# std::ranges::filter_view<V,Pred>::*iterator*

```cpp
class /*iterator*/;  // (since C++20) (exposition only*)
```

The return type of `filter_view::begin`.

This is a `bidirectional_iterator` if `V` models `bidirectional_range`, a
`forward_iterator` if `V` models `forward_range`, and `input_iterator`
otherwise.

Modification of the element denoted by this iterator is permitted, but results
in undefined behavior if the resulting value does not satisfy the filter's
predicate.

### Member types

- **`iterator_concept`** — `std::bidirectional_iterator_tag`, if `V` models
  `bidirectional_range`, `std::forward_iterator_tag`, if `V` models
  `forward_iterator`, `std::input_iterator_tag` otherwise.
- **`iterator_category`** — Defined if and only if `V` models `forward_range`.
  Let `C` be the type
  `std::iterator_traits<ranges::iterator_t<V>>::iterator_category`.
  `std::bidirectional_iterator_tag`, if `C` models
  `std::derived_from<std::bidirectional_iterator_tag>`,
  `std::forward_iterator_tag`, if `C` models
  `std::derived_from<std::forward_iterator_tag>`, `C` otherwise.
- **`value_type`** — `ranges::range_value_t<V>`
- **`difference_type`** — `ranges::range_difference_t<V>`

### Data members

- **`current_` (private)** — An iterator of type `ranges::iterator_t<V>` into
  the underlying `view`. (exposition-only member object*)
- **`parent_` (private)** — A pointer of type `ranges::filter_view<V, Pred>*` to
  the parent `filter_view` object. (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **base (C++20)** — returns the underlying iterator (public member function)
- **operator*operator-> (C++20)** — forwards to the underlying iterator (public
  member function)
- **operator++operator++(int) (C++20)** — advances the iterator (public member
  function)
- **operator--operator--(int) (C++20)** — decrements the iterator (public member
  function)

## std::ranges::filter_view::*iterator*::*iterator*

```cpp
/*iterator*/()
  requires std::default_initializable<ranges::iterator_t<V>> = default;  // (1) (since C++20)
constexpr /*iterator*/( filter_view& parent,
                        ranges::iterator_t<V> current );  // (2) (since C++20)
```

1) Initializes `current_` and `parent_` with their default member initializers,
   which are `= ranges::iterator_t<V>()` and `= nullptr` respectively.

2) Initializes `current_` with `std::move(current)` and `parent_` with
   `std::addressof(parent)`.

## std::ranges::filter_view::*iterator*::base

```cpp
constexpr const ranges::iterator_t<V>& base() const & noexcept;  // (1) (since C++20)
constexpr ranges::iterator_t<V> base() &&;  // (2) (since C++20)
```

1) Equivalent to `return current_;`.

2) Equivalent to `return std::move(current_);`.

## std::ranges::filter_view::*iterator*::operator*,->

```cpp
constexpr ranges::range_reference_t<V> operator*() const;  // (1) (since C++20)
constexpr ranges::iterator_t<V> operator->() const
  requires /*has-arrow*/<ranges::iterator_t<V>> &&
           std::copyable<ranges::iterator_t<V>>;  // (2) (since C++20)
```

1) Equivalent to `return *current_;`.

2) Equivalent to `return current_;`. For a type `I`, `/*has-arrow*/<I>` is
   modeled or satisfied, if and only if `I` models or satisfies `input_iterator`
   respectively, and either `I` is a pointer type or `requires(I i){
   i.operator->();`} is `true`.

## std::ranges::filter_view::*iterator*::operator++

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++20)
constexpr void operator++( int );  // (2) (since C++20)
constexpr /*iterator*/ operator++( int )
  requires ranges::forward_range<V>;  // (3) (since C++20)
```

1) Equivalent to `current_ = ranges::find_if(std::move(++current_),
   ranges::end(parent_->base_), std::ref(*parent_->pred_)); return *this;`.

2) Equivalent to `++*this;`.

3) Equivalent to `auto tmp = *this; ++*this; return tmp;`.

## std::ranges::filter_view::*iterator*::operator--

```cpp
constexpr /*iterator*/& operator--()
  requires ranges::bidirectional_range<V>;  // (1) (since C++20)
constexpr /*iterator*/ operator--( int )
  requires ranges::bidirectional_range<V>;  // (2) (since C++20)
```

1) Equivalent to `do --current_; while (!std::invoke(*parent_->pred_,
   *current_)); return *this;`.

2) Equivalent to `auto tmp = *this; --*this; return tmp;`.

### Non-member functions

- **operator== (C++20)** — compares the underlying iterators (function)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function)

## operator==(std::ranges::filter_view::*iterator*)

```cpp
friend constexpr bool operator==( const /*iterator*/& x, const /*iterator*/& y )
  requires std::equality_comparable<ranges::iterator_t<V>>;  // (since C++20)
```

Equivalent to `return x.current_ == y.current_;`.

The `!=` operator is synthesized from `operator==`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::filter_view::iterator` is an associated class of the arguments.

## iter_move(std::ranges::filter_view::*iterator*)

```cpp
friend constexpr ranges::range_rvalue_reference_t<V>
  iter_move( const /*iterator*/& i )
    noexcept(noexcept(ranges::iter_move(i.current_)));  // (since C++20)
```

Equivalent to `return ranges::iter_move(i.current_);`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::filter_view::iterator` is an associated class of the arguments.

## iter_swap(std::ranges::filter_view::*iterator*)

```cpp
friend constexpr void iter_swap( const /*iterator*/& x, const /*iterator*/& y )
  noexcept(noexcept(ranges::iter_swap(x.current_, y.current_)))
    requires std::indirectly_swappable<ranges::iterator_t<V>>;  // (since C++20)
```

Equivalent to `ranges::iter_swap(x.current_, y.current_)`.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when
`std::ranges::filter_view::iterator` is an associated class of the arguments.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | member type `iterator_category` was always defined | defined
      only if `V` is a `forward_range`
  LWG 3533 | C++20 | the `const&` overload of `base` copied the underlying
      iterator | returns a reference to it
  LWG 3593 | C++20 | the `const&` overload of `base` might not be noexcept |
      made noexcept

---
*Source: https://en.cppreference.com/w/cpp/ranges/filter_view/iterator*
