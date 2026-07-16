# std::packaged_task<R(Args...)>::~packaged_task

```cpp
~packaged_task();
```

Abandons the shared state and destroys the stored task object.

As with `std::promise::~promise`, if the shared state is abandoned before it was
made ready, an `std::future_error` exception is stored with the error code
`std::future_errc::broken_promise`).

### Parameters

(none)

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/%7Epackaged_task*
