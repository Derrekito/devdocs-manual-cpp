# operator==(std::expected)

```cpp
template< class T2, class E2 >
  requires (!std::is_void_v<T2>)
friend constexpr bool operator==( const expected& lhs,
                                  const std::expected<T2, E2>& rhs );  // (1) (since C++23) (T is not cv void)
template< class T2, class E2 >
  requires std::is_void_v<T2>
friend constexpr bool operator==( const expected& lhs,
                                  const std::expected<T2, E2>& rhs );  // (2) (since C++23) (T is cv void)
template< class T2 >
friend constexpr bool operator==( const expected& x, const T2& val );  // (3) (since C++23) (T is not cv void)
template< class E2 >
friend constexpr bool operator==( const expected& x,
                                  const unexpected<E2>& e );  // (4) (since C++23)
```

Performs comparison operations on `expected` objects.

1,2) Compares two `expected` objects. The objects compare equal if and only if
`lhs.has_value()` and `rhs.has_value()` are equal, and the contained values are
equal.

- For overload (1), if the expressions `*lhs == *rhs` and `lhs.error() ==
  rhs.error()` are not well-formed, or if their results are not convertible to
  `bool`, the program is ill-formed.
- For overload (2), if the expressions `lhs.error() == rhs.error()` is not
  well-formed, or if its result is not convertible to `bool`, the program is
  ill-formed.

3) Compares `expected` object with a value. The objects compare equal if and
only if `x` contains an expected value, and the contained value is equal to
`val`.

- If the expression `*x == val` is not well-formed, or if its result is not
  convertible to `bool`, the program is ill-formed.

4) Compares `expected` object with an unexpected value. The objects compare
equal if and only if `x` contains an unexpected value, and the contained value
is equal to `e.error()`.

- If the expression `x.error() == e.error()` is not well-formed, or if its
  result is not convertible to `bool`, the program is ill-formed.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::expected<T, E>` is an
associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **lhs, rhs, x** — `expected` object to compare
- **val** — value to compare to the expected value contained in `x`
- **e** — value to compare to the unexpected value contained in `x`

### Return value

1) If `lhs.has_value() != rhs.has_value()`, returns `false`. Otherwise, if
   `lhs.has_value()` is true, returns `*lhs == *rhs`. Otherwise, returns
   `lhs.error() == rhs.error()`.

2) If `lhs.has_value() != rhs.has_value()`, returns `false`. Otherwise, if
   `lhs.has_value()` is true, returns `true`. Otherwise, returns `lhs.error() ==
   rhs.error()`.

3) Returns `x.has_value() && static_cast<bool>(*x == val)`.

4) Returns `!x.has_value() && static_cast<bool>(x.error() == e.error())`.

### Exceptions

Throws when and what the comparison throws.

### Example

### See also

- **unexpected (C++23)** — represented as an unexpected value (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/operator_cmp*
