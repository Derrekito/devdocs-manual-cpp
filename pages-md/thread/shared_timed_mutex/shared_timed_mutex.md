# std::shared_timed_mutex::shared_timed_mutex

```cpp
shared_timed_mutex();  // (1) (since C++14)
shared_timed_mutex( const shared_timed_mutex& ) = delete;  // (2) (since C++14)
```

1) Constructs the mutex. The mutex is in unlocked state after the call.

2) Copy constructor is deleted.

### Parameters

(none)

### Exceptions

`std::system_error` if the construction is unsuccessful.

### See also

**C documentation for `mtx_init`**

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_timed_mutex/shared_timed_mutex*
