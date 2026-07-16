# std::recursive_mutex::recursive_mutex

```cpp
recursive_mutex();  // (1) (since C++11)
recursive_mutex( const recursive_mutex& ) = delete;  // (2) (since C++11)
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
*Source: https://en.cppreference.com/w/cpp/thread/recursive_mutex/recursive_mutex*
