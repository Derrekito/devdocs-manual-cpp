# std::expected<T,E>::~expected

```cpp
constexpr ~expected();  // (since C++23)
```

Destroys the currently contained value. That is, if `has_value()` is false,
destroys the unexpected value; otherwise, if `T` is not (possibly cv-qualified)
`void`, destroys the expected value.

This destructor is trivial if

- either `T` is (possibly cv-qualified) `void`, or
  `std::is_trivially_destructible_v<T>` is true, and
- `std::is_trivially_destructible_v<E>` is true.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/%7Eexpected*
