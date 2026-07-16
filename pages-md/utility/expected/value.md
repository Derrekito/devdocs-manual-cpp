# std::expected<T,E>::value

```cpp
void
constexpr T& value() &;  // (1) (since C++23)
constexpr const T& value() const&;  // (2) (since C++23)
constexpr T&& value() &&;  // (3) (since C++23)
constexpr const T&& value() const&&;  // (4) (since C++23)
void
constexpr void value() const&;  // (5) (since C++23)
constexpr void value() &&;  // (6) (since C++23)
```

If `*this` contains an expected value, returns a reference to the contained
value. Returns nothing if `T` is (possibly cv-qualified) void.

Otherwise, throws an exception of type std::bad_expected_access<std::decay_t<E>>
that contains a copy of `error()`.

1,2) If `std::is_copy_constructible_v<E>` is `false`, the program is ill-formed.

3,4) If `std::is_copy_constructible_v<E>` or `std::is_constructible_v<E,
   decltype(std::move(error()))>` is `false`, the program is ill-formed.

5) If `std::is_copy_constructible_v<E>` is `false`, the program is ill-formed.

6) If `std::is_move_constructible_v<E>` is `false`, the program is ill-formed.

### Parameters

(none)

### Return value

1-4) The expected value contained in `*this`.

5,6) (none)

### Exceptions

1,2,5) Throws `std::bad_expected_access(std::as_const(error()))` if `*this`
   contains an unexpected value.

3,4,6) Throws `std::bad_expected_access(std::move(error()))` if `*this` contains
   an unexpected value.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3940 | C++23 | `E` was not required to be copy/move-constructible for
      overloads (5,6) | required

### See also

- **value_or** — returns the expected value if present, another value otherwise
  (public member function)
- **operator->operator*** — accesses the expected value (public member function)
- **error** — returns the unexpected value (public member function)
- **bad_expected_access (C++23)** — exception indicating checked access to an
  `expected` that contains an unexpected value (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/value*
