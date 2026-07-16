# std::ranges::views::iota, std::ranges::iota_view

```cpp
template< std::weakly_incrementable W,
          std::semiregular Bound = std::unreachable_sentinel_t >
    requires __WeaklyEqualityComparableWith<W, Bound> && std::copyable<W>
class iota_view
    : public ranges::view_interface<iota_view<W, Bound>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ iota = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< class W >
    requires /* see below */
constexpr /* see below */ iota( W&& value );  // (since C++20)
template< class W, class Bound >
    requires /* see below */
constexpr /* see below */ iota( W&& value, Bound&& bound );  // (since C++20)
```

1) A range factory that generates a sequence of elements by repeatedly
   incrementing an initial value. Can be either bounded or unbounded (infinite).

2) `views::iota(e)` and `views::iota(e, f)` are expression-equivalent to
   `iota_view(e)` and `iota_view(e, f)` respectively for any suitable
   subexpressions `e` and `f`.

### Customization point objects

The name `views::iota` denotes a *customization point object*, which is a const
function object of a literal `semiregular` class type. For exposition purposes,
the cv-unqualified version of its type is denoted as `__iota_fn`.

All instances of `__iota_fn` are equal. The effects of invoking different
instances of type `__iota_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `views::iota` can be copied freely and its copies can be
used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::iota` above, `__iota_fn` models

- `std::invocable<__iota_fn, Args...>`,
- `std::invocable<const __iota_fn, Args...>`,
- `std::invocable<__iota_fn&, Args...>`, and
- `std::invocable<const __iota_fn&, Args...>`.

Otherwise, no function call operator of `__iota_fn` participates in overload
resolution.

### Data members

- **`value_` (private)** — The beginning value of type `W`. (exposition-only
  member object*)
- **`bound_` (private)** — The sentinel value of type `Bound`. (exposition-only
  member object*)

### Member functions

- **(constructor) (C++20)** — creates an `iota_view` (public member function)
- **begin (C++20)** — obtains the beginning iterator of an `iota_view` (public
  member function)
- **end (C++20)** — obtains the sentinel denoting the end of an `iota_view`
  (public member function)
- **empty (C++20)** — tests whether the `iota_view` is empty, i.e. the iterator
  and the sentinel compare equal (public member function)
- **size (C++20)** — obtains the size of an `iota_view` if it is sized (public
  member function)

**Inherited from `std::ranges::view_interface`**

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

## std::ranges::iota_view::iota_view

```cpp
iota_view() requires std::default_initializable<W> = default;  // (1) (since C++20)
constexpr explicit iota_view( W value );  // (2) (since C++20)
constexpr explicit iota_view( std::type_identity_t<W> value,
                              std::type_identity_t<Bound> bound );  // (3) (since C++20)
constexpr explicit iota_view( /* iterator */ first, /* see below */ last );  // (4) (since C++20)
```

1) Value-initializes `value_` and `bound_` via their default member initializers
   (`= W()` and `= Bound()`).

2) Initializes `value_` with `value` and value-initializes `bound_`. This
   constructor is used to create unbounded `iota_view`s, e.g. `iota(0)` yields
   numbers 0,1,2..., infinitely.

3) Initializes `value_` with `value` and `bound_` with `bound`. The behavior is
   undefined if `std::totally_ordered_with<W, Bound>` is modeled and `bool(value
   <= bound)` is `false`. This constructor is used to create bounded iota views,
   e.g. `iota(10, 20)` yields numbers from 10 to 19.
4) Same as (3), except that `value_` is initialized with the `W` value stored in
`first`, and

- if `W` and `Bound` are the same type, then the type of `last` is `/* iterator
  */` and `bound_` initialized with the `W` value stored in `last`,
- otherwise, if the `iota_view` is unbounded (i.e. `Bound` is
  `std::unreachable_sentinel_t`), then the type of `last` is
  `std::unreachable_sentinel_t` and `bound_` initialized with
  `std::unreachable_sentinel`.
- otherwise, the type of `last` is `/* sentinel */` and `bound_` initialized
  with the `Bound` value stored in `last`.

In any case, the type of `last` is same as `decltype(end())`.

For (2), (3), and (4), the behavior is undefined if the `iota_view` is bounded
(i.e. `Bound` is not `std::unreachable_sentinel_t`) and `bound_` is initialized
to a value unreachable from `value_`.

### Parameters

- **value** — the starting value
- **bound** — the bound
- **first** — the iterator denoting the starting value
- **last** — the iterator or sentinel denoting the bound

## std::ranges::iota_view::begin

```cpp
constexpr /* iterator */ begin() const;  // (since C++20)
```

Returns an iterator initialized with `value_`.

## std::ranges::iota_view::end

```cpp
constexpr auto end() const;  // (1) (since C++20)
constexpr /* iterator */ end() const requires std::same_as<W, Bound>;  // (2) (since C++20)
```

1) Returns a sentinel of a specific type (shown as `/* sentinel */` here)
   initialized with `bound_` if this view is bounded, or
   `std::unreachable_sentinel` if this view is unbounded.

2) Returns an iterator initialized with `bound_`.

## std::ranges::iota_view::empty

```cpp
constexpr bool empty() const;  // (since C++20)
```

Equivalent to `return value_ == bound_;`.

## std::ranges::iota_view::size

```cpp
constexpr auto size() const
    requires (std::same_as<W, Bound> && /* advanceable */<W>)
        || (/* is-integer-like */<W> && /* is-integer-like */<Bound>)
        || std::sized_sentinel_for<Bound, W>
{
    if constexpr (/* is-integer-like */<W> && /* is-integer-like */<Bound>)
        return (value_ < 0)
            ? ((bound_ < 0)
                ? /* to-unsigned-like */(-value_)
                    - /* to-unsigned-like */(-bound_)
                : /* to-unsigned-like */(bound_)
                    + /* to-unsigned-like */(-value_))
            : /* to-unsigned-like */(bound_) - /* to-unsigned-like */(value_);
    else
        return /* to-unsigned-like */(bound_ - value_);
}  // (since C++20)
```

Returns the size of the view if the view is bounded.

The exposition-only concept `advanceable` is described in this page.

The exposition-only function template `to-unsigned-like` converts its argument
(which must be integer-like) to the corresponding unsigned version of the
argument type.

### Deduction guides

```cpp
template< class W, class Bound >
    requires (!/* is-integer-like */<W>
        || !/* is-integer-like */<Bound>
        || /* is-signed-integer-like */<W>
            == /* is-signed-integer-like */<Bound>)
  iota_view( W, Bound ) -> iota_view<W, Bound>;  // (since C++20)
```

For any type `T`, /* is-integer-like */<T> is `true` if and only if `T` is
integer-like, and /* is-signed-integer-like */<T> is `true` if and only if `T`
is integer-like and capable of representing negative values.

Note that the guide protects itself against signed/unsigned mismatch bugs, like
`views::iota(0, v.size())`, where `​0​` is a (signed) `int` and `v.size()` is an
(unsigned) `std::size_t`.

### Nested classes

- ***iterator* (C++20)** — the iterator type (exposition-only member class*)
- ***sentinel* (C++20)** — the sentinel type used when the `iota_view` is
  bounded and `Bound` and `W` are not the same type (exposition-only member
  class*)

### Helper templates

```cpp
template< std::weakly_incrementable W, std::semiregular Bound >
inline constexpr bool enable_borrowed_range<ranges::iota_view<W, Bound>> = true;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `iota_view`
satisfy `borrowed_range`.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

struct Bound
{
    int bound;
    bool operator==(int x) const { return x == bound; }
};

int main()
{
    for (int i : std::ranges::iota_view{1, 10})
        std::cout << i << ' ';
    std::cout << '\n';

    for (int i : std::views::iota(1, 10))
        std::cout << i << ' ';
    std::cout << '\n';

    for (int i : std::views::iota(1, Bound{10}))
        std::cout << i << ' ';
    std::cout << '\n';

    for (int i : std::views::iota(1) | std::views::take(9))
        std::cout << i << ' ';
    std::cout << '\n';

    std::ranges::for_each(std::views::iota(1, 10), [](int i)
    {
        std::cout << i << ' ';
    });
    std::cout << '\n';
}
```

Output:

```text
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3523 | C++20 | iterator-sentinel pair constructor might use wrong sentinel
      type | corrected
  LWG 3610 | C++20 | `size` might reject integer-class types | accept if
      possible
  LWG 4001 | C++20 | the inherited member `empty` function was not always valid
      | `empty` is always provided
  P2325R3 | C++20 | `iota_view` required that `W` is `semiregular` as `view`
      required `default_initializable` | only requires that `W` is `copyable`
  P2711R1 | C++20 | the multi-parameter constructors were not explicit | made
      explicit

### See also

- **iota (C++11)** — fills a range with successive increments of the starting
  value (function template)
- **ranges::iota (C++23)** — fills a range with successive increments of the
  starting value (niebloid)
- **ranges::repeat_viewviews::repeat (C++23)** — a `view` consisting of a
  generated sequence by repeatedly producing the same value (class template)
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/iota_view*
