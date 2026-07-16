# std::weak_ordering

```cpp
class weak_ordering;  // (since C++20)
```

The class type `std::weak_ordering` is the result type of a three-way comparison
that:

- Admits all six relational operators (`==`, `!=`, `<`, `<=`, `>`, `>=`).
- Does not imply substitutability: if `a` is equivalent to `b`, `f(a)` may not
  be equivalent to `f(b)`, where `f` denotes a function that reads only
  comparison-salient state that is accessible via the argument's public const
  members. In other words, equivalent values may be distinguishable.
- Does not allow incomparable values: exactly one of `a < b`, `a == b`, or `a >
  b` must be `true`.

### Constants

The type `std::weak_ordering` has three valid values, implemented as const
static data members of its type:

- **less(inline constexpr) [static]** — a valid value of the type
  `std::weak_ordering` indicating less-than (ordered before) relationship
  (public static member constant)
- **equivalent(inline constexpr) [static]** — a valid value of the type
  `std::weak_ordering` indicating equivalence (neither ordered before nor
  ordered after) (public static member constant)
- **greater(inline constexpr) [static]** — a valid value of the type
  `std::weak_ordering` indicating greater-than (ordered after) relationship
  (public static member constant)

### Conversions

`std::weak_ordering` is implicitly-convertible to `std::partial_ordering`, while
`std::strong_ordering` is implicitly-convertible to `weak_ordering`.

- ****operator partial_ordering**** — implicit conversion to
  `std::partial_ordering` (public member function)

## std::weak_ordering::operator partial_ordering

```cpp
constexpr operator partial_ordering() const noexcept;
```

### Return value

`std::partial_ordering::less` if `v` is `less`, `std::partial_ordering::greater`
if `v` is `greater`, `std::partial_ordering::equivalent` if `v` is `equivalent`.

### Comparisons

Comparison operators are defined between values of this type and literal `​0​`.
This supports the expressions `a <=> b == 0` or `a <=> b < 0` that can be used
to convert the result of a three-way comparison operator to a boolean
relationship; see `std::is_eq`, `std::is_lt`, etc.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::weak_ordering` is an
associated class of the arguments.

The behavior of a program that attempts to compare a `weak_ordering` with
anything other than the integer literal `​0​` is undefined.

- ****operator==operator<operator>operator<=operator>=operator<=>**** — compares
  with zero or a `weak_ordering` (function)

## operator==

```cpp
friend constexpr bool operator==( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr bool operator==( weak_ordering v, weak_ordering w ) noexcept = default;  // (2)
```

### Parameters

- **v, w** — `std::weak_ordering` values to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `true` if `v` is `equivalent`, `false` if `v` is `less` or `greater`

2) `true` if both parameters hold the same value, `false` otherwise

## operator<

```cpp
friend constexpr bool operator<( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr bool operator<( /*unspecified*/ u, weak_ordering v ) noexcept;  // (2)
```

### Parameters

- **v** — a `std::weak_ordering` value to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `true` if `v` is `less`, and `false` if `v` is `greater` or `equivalent`

2) `true` if `v` is `greater`, and `false` if `v` is `less` or `equivalent`

## operator<=

```cpp
friend constexpr bool operator<=( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr bool operator<=( /*unspecified*/ u, weak_ordering v ) noexcept;  // (2)
```

### Parameters

- **v** — a `std::weak_ordering` value to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `true` if `v` is `less` or `equivalent`, and `false` if `v` is `greater`

2) `true` if `v` is `greater` or `equivalent`, and `false` if `v` is `less`

## operator>

```cpp
friend constexpr bool operator>( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr bool operator>( /*unspecified*/ u, weak_ordering v ) noexcept;  // (2)
```

### Parameters

- **v** — a `std::weak_ordering` value to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `true` if `v` is `greater`, and `false` if `v` is `less` or `equivalent`

2) `true` if `v` is `less`, and `false` if `v` is `greater` or `equivalent`

## operator>=

```cpp
friend constexpr bool operator>=( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr bool operator>=( /*unspecified*/ u, weak_ordering v ) noexcept;  // (2)
```

### Parameters

- **v** — a `std::weak_ordering` value to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `true` if `v` is `greater` or `equivalent`, and `false` if `v` is `less`

2) `true` if `v` is `less` or `equivalent`, and `false` if `v` is `greater`

## operator<=>

```cpp
friend constexpr weak_ordering operator<=>( weak_ordering v, /*unspecified*/ u ) noexcept;  // (1)
friend constexpr weak_ordering operator<=>( /*unspecified*/ u, weak_ordering v ) noexcept;  // (2)
```

### Parameters

- **v** — a `std::weak_ordering` value to check
- **u** — an unused parameter of any type that accepts literal zero argument

### Return value

1) `v`.

2) `greater` if `v` is `less`, `less` if `v` is `greater`, otherwise `v`.

### Example

### See also

- **strong_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators and is substitutable (class)
- **partial_ordering (C++20)** — the result type of 3-way comparison that
  supports all 6 operators, is not substitutable, and allows incomparable values
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/weak_ordering*
