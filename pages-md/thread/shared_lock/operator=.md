# std::shared_lock<Mutex>::operator=

```cpp
shared_lock& operator=( shared_lock&& other ) noexcept;  // (since C++14)
```

Move assignment operator. Replaces the contents with those of `other` using move
semantics.

If, prior to this call, `*this` has an associated mutex ((`mutex()` returns a
non-null pointer) and has acquired ownership of it (`owns()` returns `true`),
the mutex is unlocked by calling `unlock_shared()`.

After this call, `other` has no associated mutex.

### Parameters

- **other** — another `shared_lock` to replace the state with

### Return value

`*this`

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/operator%3D*
