# std::coroutine_handle<Promise>::operator coroutine_handle<>

```cpp
constexpr operator coroutine_handle<>() const noexcept;  // (since C++20)
```

This conversion function converts a `std::coroutine_handle<Promise>` value to a
`std::coroutine_handle<>` holding the same underlying address. It effectively
erases the promise type.

### Parameters

(none)

### Return value

`std::coroutine_handle<>::from_address(address())`

### See also

- **operator==operator<=> (C++20)** — compares two `coroutine_handle` objects
  (function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/operator_coroutine_handle_void*
