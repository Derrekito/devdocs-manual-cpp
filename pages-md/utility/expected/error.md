# std::expected<T,E>::error

```cpp
constexpr const E& error() const& noexcept;  // (1) (since C++23)
constexpr E& error() & noexcept;  // (2) (since C++23)
constexpr const E&& error() const&& noexcept;  // (3) (since C++23)
constexpr E&& error() && noexcept;  // (4) (since C++23)
```

Accesses the unexpected value contained in `*this`.

The behavior is undefined if `this->has_value()` is `true`.

### Parameters

(none)

### Return value

Reference to the unexpected value contained in `*this`.

### Example

### See also

- **operator->operator*** — accesses the expected value (public member function)
- **value** — returns the expected value (public member function)
- **operator boolhas_value** — checks whether the object contains an expected
  value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/error*
