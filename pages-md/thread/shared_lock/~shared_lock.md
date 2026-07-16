# std::shared_lock<Mutex>::~shared_lock

```cpp
~shared_lock();  // (since C++14)
```

Destroys the lock.

If `*this` has an associated mutex ((`mutex()` returns a non-null pointer) and
has acquired ownership of it (`owns()` returns `true`), the mutex is unlocked by
calling `unlock_shared()`.

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/%7Eshared_lock*
