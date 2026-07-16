# std::bit_not<void>

```cpp
template<>
class bit_not<void>;  // (since C++14)
```

std::bit_not<void> is a specialization of `std::bit_not` with parameter and
return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — applies `operator~` to its argument (public member
  function)

## std::bit_not<void>::operator()

```cpp
template< class T >
constexpr auto operator()( T&& arg ) const
    -> decltype(~std::forward<T>(arg));
```

Returns the result of `~std::forward<T>(arg)`.

### Parameters

- **arg** — value to bitwise NOT

### Return value

`~std::forward<T>(arg)`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/bit_not_void*
