# std::expected<T,E>::value_or

```cpp
template< class U >
constexpr T value_or( U&& default_value ) const&;  // (1) (since C++23)
template< class U >
constexpr T value_or( U&& default_value ) &&;  // (2) (since C++23)
```

Returns the contained value if `*this` contains an expected value, otherwise
returns `default_value`.

1) Returns `bool(*this) ? **this :
   static_cast<T>(std::forward<U>(default_value))`

2) Returns `bool(*this) ? std::move(**this) :
   static_cast<T>(std::forward<U>(default_value))`

### Parameters

- **default_value** — the value to use in case `*this` does not contain an
  expected value

**Type requirements**

**-`T` must meet the requirements of CopyConstructible in order to use overload (1).**

**-`T` must meet the requirements of MoveConstructible in order to use overload (2).**

**-`U&&` must be convertible to `T`**

### Return value

The currently contained value if `*this` contains an expected value, or
`default_value` otherwise.

### Exceptions

Any exception thrown by the selected constructor of the return value `T`.

### Notes

If `T` is (possibly cv-qualified) `void`, this member is not declared.

### Example

### See also

- **value** — returns the expected value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/value_or*
