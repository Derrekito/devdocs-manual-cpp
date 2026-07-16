# std::unique_lock<Mutex>::~unique_lock

```cpp
~unique_lock();  // (since C++11)
```

Destroys the lock. If `*this` has an associated mutex and has acquired ownership
of it, the mutex is unlocked.

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/%7Eunique_lock*
