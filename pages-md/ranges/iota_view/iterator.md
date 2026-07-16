# std::ranges::iota_view<W, Bound>::*iterator*

```cpp
struct /*iterator*/;  // (1) (since C++20) (exposition only*)
Helper alias templates
template< class I >
using /*iota-diff-t*/ = /* see below */;  // (2) (exposition only*)
Helper concepts
template< class I >
concept /*decrementable*/ =
  std::incrementable<I> && requires(I i) {
    { --i } -> std::same_as<I&>;
    { i-- } -> std::same_as<I>;
  };  // (3) (exposition only*)
template< class I >
concept /*advanceable*/ =
  /*decrementable*/<I> && std::totally_ordered<I> &&
  requires(I i, const I j, const /*iota-diff-t*/<I> n) {
    { i += n } -> std::same_as<I&>;
    { i -= n } -> std::same_as<I&>;
    I(j + n);
    I(n + j);
    I(j - n);
    { j - j } -> std::convertible_to</*iota-diff-t*/<I>>;
  };  // (4) (exposition only*)
```

1) The return type of `iota_view::begin`.
2) The alias template `/*iota-diff-t*/` calculates the difference type for both
iterator types and integer-like types.

- If `W` is not an integral type, or if it is an integral type and
  `sizeof(std::iter_difference_t<I>)` is greater than `sizeof(I)`, then
  `/*iota-diff-t*/<I>` is `std::iter_difference_t<I>`.
- Otherwise, `/*iota-diff-t*/<I>` is a signed integer type of width greater than
  the width of `I` if such a type exists.
- Otherwise, `I` is one of the widest integral types, and `/*iota-diff-t*/<I>`
  is an unspecified signed-integer-like type of width not less than the width of
  `I`. It is unspecified whether `/*iota-diff-t*/<I>` models
  `weakly_incrementable` in this case.

3) The concept `decrementable` specifies that a type is `incrementable`, and
   pre- and post- `operator--` for the type have common meaning.

4) The concept `advanceable` specifies that a type is both `decrementable` and
   `totally_ordered`, and `operator+=`, `operator-=`, `operator+`, and
   `operator-` among the type and its different type have common meaning.

### Semantic requirements

3) Type `I` models `decrementable` only if `I` satisfies `decrementable` and all
concepts it subsumes are modeled, and given equal objects `a` and `b` of type
`I`:

- If `a` and `b` are in the domain of both pre- and post- `operator--` (i.e.
  they are decrementable), then the following are all `true`:
  `std::addressof(--a) == std::addressof(a)`, `bool(a-- == b)`,
  `bool(((void)a--, a) == --b)`, `bool(++(--a) == b)`.
- If `a` and `b` are in the domain of both pre- and post- `operator++` (i.e.
  they are incrementable), then `bool(--(++a) == b)` is `true`.

4) Let `D` denote `/*iota-diff-t*/<I>`. Type `I` models `advanceable` only if
`I` satisfies `advanceable` and all concepts it subsumes are modeled, and given

- objects `a` and `b` of type `I` and
- value `n` of type `D`,

such that `b` is reachable from `a` after `n` applications of `++a`, all
following conditions are satisfied:

- `(a += n)` is equal to `b`.
- `std::addressof(a += n)` is equal to `std::addressof(a)`.
- `I(a + n)` is equal to `(a += n)`.
- For any two positive values `x` and `y` of type `D`, if `I(a + D(x + y))` is
  well-defined, then `I(a + D(x + y))` is equal to `I(I(a + x) + y)`.
- `I(a + D(0))` is equal to `a`.
- If `I(a + D(n - 1))` is well-defined, then `I(a + n)` is equal to `[](I c) {
  return ++c; }(I(a + D(n - 1)))`.
- `(b += -n)` is equal to `a`.
- `(b -= n)` is equal to `a`.
- `std::addressof(b -= n)` is equal to `std::addressof(b)`.
- `I(b - n)` is equal to `(b -= n)`.
- `D(b - a)` is equal to `n`.
- `D(a - b)` is equal to `D(-n)`.
- `bool(a <= b)` is `true`.

### Member types

- **`iterator_concept`** — `std::random_access_iterator_tag` if `W` models
  `advanceable`. Otherwise, `std::bidirectional_iterator_tag` if `W` models
  `decrementable`. Otherwise, `std::forward_iterator_tag` if `W` models
  `incrementable`. Otherwise, `std::input_iterator_tag`.
- **`iterator_category`** — `std::input_iterator_tag` if `W` models
  `incrementable`. Otherwise, there is no member type `iterator_category`.
- **`value_type`** — `W`
- **`difference_type`** — `/*iota-diff-t*/<W>`

Notes: `/*iterator*/` is

- `random_access_iterator` if `W` models `advanceable`,
- `bidirectional_iterator` if `W` models `decrementable`,
- `forward_iterator` if `W` models `incrementable`, and
- `input_iterator` otherwise.

However, it only satisfies LegacyInputIterator if `W` models `incrementable`,
and does not satisfy LegacyInputIterator otherwise.

### Data members

- **`value_` (private)** — The value of type `W` used for dereferencing.
  (exposition-only member object*)

### Member functions

## std::ranges::iota_view::*iterator*::*iterator*

```cpp
/*iterator*/() requires std::default_initializable<W> = default;  // (1) (since C++20)
constexpr explicit /*iterator*/( W value );  // (2) (since C++20)
```

1) Value initializes the data member `value_` via its default member initializer
   (`= W()`).

2) Initializes the data member `value_` with `value`. This value will be
   returned by `operator*` and incremented by `operator++`.

## std::ranges::iota_view::*iterator*::operator*

```cpp
constexpr W operator*() const
    noexcept(std::is_nothrow_copy_constructible_v<W>);  // (since C++20)
```

Returns the current value, by value (in other words, this is a read-only view).

## std::ranges::iota_view::*iterator*::operator++

```cpp
constexpr /*iterator*/& operator++();  // (1) (since C++20)
constexpr void operator++(int);  // (2) (since C++20)
constexpr /*iterator*/ operator++(int) requires std::incrementable<W>;  // (3) (since C++20)
```

1) Equivalent to `++value_; return *this;`.

2) Equivalent to `++value_;`.

3) Equivalent to `auto tmp = *this; ++value_; return tmp;`.

## std::ranges::iota_view::*iterator*::operator--

```cpp
constexpr /*iterator*/& operator--() requires /*decrementable*/<W>;  // (1) (since C++20)
constexpr /*iterator*/operator--(int) requires /*decrementable*/<W>;  // (2) (since C++20)
```

1) Equivalent to `--value_; return *this;`.

2) Equivalent to `auto tmp = *this; --value_; return tmp;`.

## std::ranges::iota_view::*iterator*::operator+=

```cpp
constexpr /*iterator*/& operator+=( difference_type n )
    requires /*advanceable*/<W>;  // (since C++20)
```

If `W` is unsigned-integer-like, performs `value_ += static_cast<W>(n)` if `n`
is non-negative, `value -= static_cast<W>(-n)` otherwise, and then returns
`*this`.

Otherwise, equivalent to `value_ += n; return *this;`.

## std::ranges::iota_view::*iterator*::operator-=

```cpp
constexpr /*iterator*/& operator-=( difference_type n )
    requires /*advanceable*/<W>;  // (since C++20)
```

If `W` is unsigned-integer-like, performs `value_ -= static_cast<W>(n)` if `n`
is non-negative, or `value += static_cast<W>(-n)` otherwise, and then returns
`*this`.

Otherwise, equivalent to `value_ -= n; return *this;`.

## std::ranges::iota_view::*iterator*::operator[]

```cpp
constexpr W operator[]( difference_type n ) const
    requires /*advanceable*/<W>;  // (since C++20)
```

Equivalent to `return W(value_ + n);`.

### Non-member functions

## operator==, <, >, <=, >=, <=>(std::ranges::iota_view::*iterator*)

```cpp
friend constexpr bool operator== ( const /*iterator*/& x, const /*iterator*/& y )
    requires std::equality_comparable<W>;  // (1) (since C++20)
friend constexpr bool operator<  ( const /*iterator*/& x, const /*iterator*/& y )
    requires std::totally_ordered<W>;  // (2) (since C++20)
friend constexpr bool operator>  ( const /*iterator*/& x, const /*iterator*/& y )
    requires std::totally_ordered<W>;  // (3) (since C++20)
friend constexpr bool operator<= ( const /*iterator*/& x, const /*iterator*/& y )
    requires std::totally_ordered<W>;  // (4) (since C++20)
friend constexpr bool operator>= ( const /*iterator*/& x, const /*iterator*/& y )
    requires std::totally_ordered<W>;  // (5) (since C++20)
friend constexpr bool operator<=>( const /*iterator*/& x, const /*iterator*/& y )
    requires std::totally_ordered<W> && std::three_way_comparable<W>;  // (6) (since C++20)
```

1) Equivalent to `return x.value_ == y.value_;`.

2) Equivalent to `return x.value_ < y.value_;`.

3) Equivalent to `return y < x;`.

4) Equivalent to `return !(y < x);`.

5) Equivalent to `return !(x < y);`.

6) Equivalent to `return x.value_ <=> y.value_;`.

The `!=` operator is synthesized from `operator==`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

## operator+(std::ranges::iota_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator+( /*iterator*/ i, difference_type n )
    requires /*advanceable*/<W>;  // (1) (since C++20)
friend constexpr /*iterator*/ operator+( difference_type n, /*iterator*/ i )
    requires /*advanceable*/<W>;  // (2) (since C++20)
```

Equivalent to `i += n; return i;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

## operator-(std::ranges::iota_view::*iterator*)

```cpp
friend constexpr /*iterator*/ operator-( /*iterator*/ i, difference_type n )
    requires /*advanceable*/<W>;  // (1) (since C++20)
friend constexpr difference_type operator-( const /*iterator*/& x,
                                            const /*iterator*/& y )
    requires /*advanceable*/<W>;  // (2) (since C++20)
```

1) Equivalent to `i -= n; return i;`.
2) Let `D` be `difference_type`.

- If `W` is signed-integer-like, equivalent to `return D(D(x.value_) -
  D(y.value_));`.
- Otherwise, if `W` is unsigned-integer-like, equivalent to `return y.value_ >
  x.value_ ? D(-D(y.value_ - x.value_)) : D(x.value_ - y.value_);`.
- Otherwise, equivalent to `return x.value_ - y.value_;`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `iterator` is an associated
class of the arguments.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | member `iterator_category` is always defined | defined only
      if `W` satisfies `incrementable`
  LWG 3580 | C++20 | bodies of `operator+` and `operator-` rule out implicit
      move | made suitable for implicit move

---
*Source: https://en.cppreference.com/w/cpp/ranges/iota_view/iterator*
