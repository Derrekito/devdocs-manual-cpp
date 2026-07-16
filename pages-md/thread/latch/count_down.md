# std::latch::count_down

```cpp
void count_down( std::ptrdiff_t n = 1 );  // (since C++20)
```

Atomically decrements the internal counter by `n` without blocking the caller.

If `n` is greater than the value of the internal counter or is negative, the
behavior is undefined.

This operation strongly happens-before all calls that are unblocked on this
`latch`.

### Parameters

- **n** — the value by which the internal counter is decreased

### Return value

(none)

### Exceptions

Throws `std::system_error` with an error code allowed for mutex types on error.

---
*Source: https://en.cppreference.com/w/cpp/thread/latch/count_down*
