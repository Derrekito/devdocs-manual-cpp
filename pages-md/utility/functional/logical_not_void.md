# std::logical_not<void>

```cpp
template<>
class logical_not<void>;  // (since C++14)
```

std::logical_not<void> is a specialization of `std::logical_not` with parameter
and return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — applies `operator!` to its argument (public member
  function)

## std::logical_not<void>::operator()

```cpp
template< class T >
constexpr auto operator()( T&& arg ) const
    -> decltype(!std::forward<T>(arg));
```

Returns the result of `!std::forward<T>(arg)`.

### Parameters

- **arg** — value to apply logical NOT to

### Return value

`!std::forward<T>(arg)`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/logical_not_void*
