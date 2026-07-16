# std::expected<T,E>::emplace

```cpp
T is not cv void
template< class... Args >
constexpr T& emplace( Args&&... args ) noexcept;  // (1) (since C++23)
template< class U, class... Args >
constexpr T& emplace( std::initializer_list<U>& il, Args&&... args ) noexcept;  // (2) (since C++23)
T is cv void
constexpr void emplace() noexcept;  // (3) (since C++23)
```

Constructs an expected value in-place. After the call, `has_value()` returns
true.

1) Destroys the contained value, then initializes the expected value contained
   in `*this` as if by direct-initializing an object of type `T` from the
   arguments `std::forward<Args>(args)...`. This overload participates in
   overload resolution only if `std::is_nothrow_constructible_v<T, Args...>` is
   true.

2) Destroys the contained value, then initializes the expected value contained
   in `*this` as if by direct-initializing an object of type `T` from the
   arguments `il, std::forward<Args>(args)...`. This overload participates in
   overload resolution only if `std::is_nothrow_constructible_v<T,
   std::initializer_list<U>&, Args...>` is true.

3) If `*this` contains an unexpected value, destroys that value.

### Parameters

- **args...** — the arguments to pass to the constructor
- **il** — the initializer list to pass to the constructor

### Return value

A reference to the new contained value.

### Exceptions

`noexcept` specification:

`noexcept`

### Notes

If the construction of `T` is potentially-throwing, this function is not
defined. In this case, it is the responsibility of the user to create a
temporary and move or copy it.

### Example

### See also

- **operator=** — assigns contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/expected/emplace*
