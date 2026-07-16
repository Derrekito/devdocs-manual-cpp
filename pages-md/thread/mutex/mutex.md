# std::mutex::mutex

```cpp
constexpr mutex() noexcept;  // (1) (since C++11)
mutex( const mutex& ) = delete;  // (2) (since C++11)
```

1) Constructs the mutex. The mutex is in unlocked state after the constructor
   completes.

2) Copy constructor is deleted.

### Parameters

(none)

### Notes

Because the default constructor is `constexpr`, static mutexes are initialized
as part of static non-local initialization, before any dynamic non-local
initialization begins. This makes it safe to lock a mutex in a constructor of
any static object.

### See also

**C documentation for `mtx_init`**

---
*Source: https://en.cppreference.com/w/cpp/thread/mutex/mutex*
