# std::not_equal_to<void>

```cpp
template<>
class not_equal_to<void>;  // (since C++14)
```

std::not_equal_to<void> is a specialization of `std::not_equal_to` with
parameter and return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — tests if the two arguments do not compare equal (public
  member function)

## std::not_equal_to<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) != std::forward<U>(rhs));
```

Returns the result of non-equality comparison between `lhs` and `rhs`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`std::forward<T>(lhs) != std::forward<U>(rhs)`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/not_equal_to_void*
