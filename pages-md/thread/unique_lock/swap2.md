# std::swap(std::unique_lock)

```cpp
template< class Mutex >
void swap( unique_lock<Mutex>& lhs,
           unique_lock<Mutex>& rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::unique_lock`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — lock wrappers whose states to swap

### Return value

(none)

### Example

### See also

- **swap** — swaps state with another `std::unique_lock` (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock/swap2*
