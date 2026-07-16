# std::latch::wait

```cpp
void wait() const;  // (since C++20)
```

Blocks the calling thread until the internal counter reaches `​0​`. If it is
zero already, returns immediately.

### Parameters

(none)

### Return value

(none)

### Exceptions

Throws `std::system_error` with an error code allowed for mutex types on error.

---
*Source: https://en.cppreference.com/w/cpp/thread/latch/wait*
