# std::coroutine_handle<Promise>::address

```cpp
constexpr void* address() const noexcept;  // (since C++20)
```

Returns the underlying address of the `coroutine_handle`. The return value is
non-null if and only if the current value of the `coroutine_handle` is obtained
from a promise object of a coroutine.

### Parameters

(none)

### Return value

The underlying address.

### Notes

The return value is non-null for specialization `std::noop_coroutine_handle`,
because a `std::noop_coroutine_handle` cannot be created without referring to a
no-op coroutine.

### See also

- **from_address [static] (C++20)** — imports a coroutine from a pointer (public
  static member function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/address*
