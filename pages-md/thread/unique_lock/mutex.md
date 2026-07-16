# std::unique_lock<Mutex>::mutex

```cpp
mutex_type* mutex() const noexcept;  // (since C++11)
```

Returns a pointer to the associated mutex, or a null pointer if there is no
associated mutex.

### Parameters

(none)

### Return value

Pointer to the associated mutex or a null pointer if there is no associated
mutex.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/mutex*
