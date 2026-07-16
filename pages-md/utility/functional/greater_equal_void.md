# std::greater_equal<void>

```cpp
template<>
class greater_equal<void>;  // (since C++14)
```

std::greater_equal<void> is a specialization of `std::greater_equal` with
parameter and return type deduced.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — tests if `lhs` compares greater or equal than `rhs`
  (public member function)

## std::greater_equal<void>::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& lhs, U&& rhs ) const
    -> decltype(std::forward<T>(lhs) >= std::forward<U>(rhs));
```

Returns the result of `std::forward<T>(lhs) >= std::forward<U>(rhs)`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`std::forward<T>(lhs) >= std::forward<U>(rhs)`.

If a built-in operator comparing pointers is called, the result is consistent
with the implementation-defined strict total order over pointers.

### Exceptions

May throw implementation-defined exceptions.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2562 | C++98 | the pointer total order might be inconsistent | guaranteed
      to be consistent

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/greater_equal_void*
