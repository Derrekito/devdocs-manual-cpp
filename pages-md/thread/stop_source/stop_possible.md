# std::stop_source::stop_possible

```cpp
[[nodiscard]] bool stop_possible() const noexcept;  // (since C++20)
```

Checks if the `stop_source` object has a stop-state.

### Parameters

(none)

### Return value

`true` if the `stop_source` object has a stop-state, otherwise `false`.

### Notes

If the `stop_source` object has a stop-state and a stop request has already been
made, this function still returns `true`.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_source/stop_possible*
