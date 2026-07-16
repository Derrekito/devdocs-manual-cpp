# std::coroutine_handle<Promise>::promise

```cpp
Member of the primary template
Promise& promise() const;  // (since C++20)
Member of specialization std::coroutine_handle<std::noop_coroutine_promise>
std::noop_coroutine_promise& promise() const noexcept;  // (since C++20)
```

Obtains a reference to the promise object.

The behavior is undefined if `*this` does not refer to a coroutine whose promise
object has not been destroyed.

This function is not provided for the specialization `std::coroutine_handle<>`.

### Parameters

(none)

### Return value

A reference to the promise object.

### Notes

The promise object of a no-op coroutine is not destroyed as long as there is
some `std::noop_coroutine_handle` referring to the coroutine.

### See also

- **from_promise [static] (C++20)** — creates a `coroutine_handle` from the
  promise object of a coroutine (public static member function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/promise*
