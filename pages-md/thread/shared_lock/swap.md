# std::shared_lock<Mutex>::swap

```cpp
template< class Mutex >
void swap( shared_lock<Mutex>& other ) noexcept;  // (since C++14)
```

Exchanges the internal states of the lock objects.

### Parameters

- **other** — the lock to swap the state with

### Return value

(none)

### Example

### See also

- **std::swap(std::shared_lock) (C++14)** — specialization of `std::swap` for
  `shared_lock` (function template)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/swap*
