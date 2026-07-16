# std::stop_source::stop_requested

```cpp
[[nodiscard]] bool stop_requested() const noexcept;  // (since C++20)
```

Checks if the `stop_source` object has a stop-state and that state has received
a stop request.

### Parameters

(none)

### Return value

`true` if the `stop_token` object has a stop-state and it has received a stop
request, `false` otherwise.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/stop_requested*
