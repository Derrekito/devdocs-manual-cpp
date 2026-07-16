# std::promise<R>::get_future

```cpp
std::future<R> get_future();  // (since C++11)
```

Returns a future object associated with the same shared state as `*this`.

An exception is thrown if `*this` has no shared state or `get_future` has
already been called. To get multiple "pop" ends of the promise-future
communication channel, use `std::future::share`.

Calls to this function do not introduce data races with calls to `set_value`,
`set_exception`, `set_value_at_thread_exit`, or `set_exception_at_thread_exit`
(but they need not synchronize with each other).

### Parameters

(none)

### Return value

A future referring to the shared state of `*this`.

### Exceptions

`std::future_error` on the following conditions:

- `*this` has no shared state. The error code is set to `no_state`.
- `get_future()` has already been called on a promise with the same shared state
  as `*this`. The error code is set to `future_already_retrieved`.

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/get_future*
