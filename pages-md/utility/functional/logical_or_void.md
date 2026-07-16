# std::logical_or<void>

```cpp
template<>
class logical_or<void>;  // (since C++14)
```

std::logical_or<void> is a specialization of `std::logical_or` with parameter
and return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — applies `operator||` to `lhs` and `rhs` (public member
  function)

## std::logical_or<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) || std::forward<U>(rhs));
```

Returns the result of `std::forward<T>(lhs) || std::forward<U>(rhs)`.

### Parameters

- **lhs, rhs** — values to logical OR

### Return value

`std::forward<T>(lhs) || std::forward<U>(rhs)`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/logical_or_void*
