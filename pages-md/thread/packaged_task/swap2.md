# std::swap(std::packaged_task)

```cpp
template< class Function, class... Args >
void swap( packaged_task<Function(Args...)> &lhs,
           packaged_task<Function(Args...)> &rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::packaged_task`. Exchanges the
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — packaged tasks whose states to swap

### Return value

(none)

### Example

### See also

- **swap** — swaps two task objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/packaged_task/swap2*
