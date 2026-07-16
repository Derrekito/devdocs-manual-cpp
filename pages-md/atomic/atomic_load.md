# std::atomic_load, std::atomic_load_explicit

```cpp
template< class T >
T atomic_load( const std::atomic<T>* obj ) noexcept;  // (1) (since C++11)
template< class T >
T atomic_load( const volatile std::atomic<T>* obj ) noexcept;  // (2) (since C++11)
template< class T >
T atomic_load_explicit( const std::atomic<T>* obj,
                        std::memory_order order ) noexcept;  // (3) (since C++11)
template< class T >
T atomic_load_explicit( const volatile std::atomic<T>* obj,
                        std::memory_order order ) noexcept;  // (4) (since C++11)
```

1,2) Atomically obtains the value pointed to by `obj` as if by `obj->load()`.

3,4) Atomically obtains the value pointed to by `obj` as if by
   `obj->load(order)`.

If order is one of `std::memory_order_release` and `std::memory_order_acq_rel`,
   the behavior is undefined.

### Parameters

- **obj** — pointer to the atomic object to modify
- **order** — the memory synchronization ordering for this operation

### Return value

The value that is held by the atomic object pointed to by `obj`.

### See also

- **load** — atomically obtains the value of the atomic object (public member
  function of `std::atomic<T>`)
- **atomic_storeatomic_store_explicit (C++11)(C++11)** — atomically replaces the
  value of the atomic object with a non-atomic argument (function template)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

- **std::atomic_load(std::shared_ptr) std::atomic_load_explicit(std::shared_ptr)
  (deprecated in C++20)** — specializes atomic operations for `std::shared_ptr`
  (function template)

**C documentation for `atomic_load, atomic_load_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_load*
