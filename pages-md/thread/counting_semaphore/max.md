# std::counting_semaphore<LeastMaxValue>::max

```cpp
constexpr std::ptrdiff_t max() noexcept;  // (since C++20)
```

Returns the internal counter's maximum possible value, which is greater than or
equal to `LeastMaxValue`.

### Return value

The internal counter's maximum possible value, as a `std::ptrdiff_t`.

### Notes

For specialization `binary_semaphore`, `LeastMaxValue` is equal to `1`.

As its name indicates, the `LeastMaxValue` is the *minimum* max value, not the
*actual* max value. Thus `max()` can yield a number larger than `LeastMaxValue`.

---
*Source: https://en.cppreference.com/w/cpp/thread/counting_semaphore/max*
