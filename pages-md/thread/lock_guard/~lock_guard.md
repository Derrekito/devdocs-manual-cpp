# std::lock_guard<Mutex>::~lock_guard

```cpp
~lock_guard();  // (since C++11)
```

Releases the ownership of the owned mutex.

Effectively calls `m.unlock()` where `m` is the mutex passed to the
`lock_guard`'s constructor.

---
*Source: https://en.cppreference.com/w/cpp/thread/lock_guard/%7Elock_guard*
