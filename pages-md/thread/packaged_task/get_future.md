# std::packaged_task<R(Args...)>::get_future

```cpp
std::future<R> get_future();  // (since C++11)
```

Returns a `future` which shares the same shared state as `*this`.

`get_future` can be called only once for each `packaged_task`.

### Parameters

(none)

### Return value

A future which shares the same shared state as `*this`.

### Exceptions

`std::future_error` on the following error conditions:

- The shared state has already been retrieved via a call to `get_future`. The
  error category is set to `future_already_retrieved`.
- `*this` has no shared state. The error category is set to `no_state`.

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/get_future*
