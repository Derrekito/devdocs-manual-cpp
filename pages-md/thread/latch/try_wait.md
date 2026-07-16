# std::latch::try_wait

```cpp
bool try_wait() const noexcept;  // (since C++20)
```

Returns `true` only if the internal counter has reached zero. This function may
spuriously return `false` with very low probability even if the internal counter
has reached zero.

### Parameters

(none)

### Return value

With very low probability `false`, otherwise `cnt == 0`, where `cnt` is the
value of the internal counter.

### Notes

The reason why a spurious result is permitted is to allow implementations to use
a memory order more relaxed than `std::memory_order_seq_cst`.

---
*Source: https://en.cppreference.com/w/cpp/thread/latch/try_wait*
