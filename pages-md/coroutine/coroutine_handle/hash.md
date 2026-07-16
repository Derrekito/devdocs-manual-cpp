# std::hash<std::coroutine_handle>

```cpp
template< class Promise >
struct hash<std::coroutine_handle<Promise>>;  // (since C++20)
```

The template specialization of `std::hash` for `std::coroutine_handle` allows
users to obtain hashes of objects of type `std::coroutine_handle<P>`.

`operator()` of the specialization is noexcept.

### Example

### See also

- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/hash*
