# std::unique_lock<Mutex>::operator=

```cpp
unique_lock& operator=( unique_lock&& other );  // (since C++11)
```

Move assignment operator. Replaces the contents with those of `other` using move
semantics.

If prior to the call `*this` has an associated mutex and has acquired ownership
of it, the mutex is unlocked.

### Parameters

- **other** — another `unique_lock` to replace the state with

### Return value

`*this`

### Exceptions

Throws nothing.

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/operator%3D*
