# std::swap(std::promise)

```cpp
template< class R >
void swap( promise<R>& lhs, promise<R>& rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::promise`. Exchanges the shared
state of `lhs` with that of `rhs`. Effectively calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — promises whose states to swap

### Return value

(none)

### Example

### See also

- **swap** — swaps two promise objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/promise/swap2*
