# std::atomic_store, std::atomic_store_explicit

```cpp
template< class T >
void atomic_store( std::atomic<T>* obj,
                   typename std::atomic<T>::value_type desired ) noexcept;  // (1) (since C++11)
template< class T >
void atomic_store( volatile std::atomic<T>* obj,
                   typename std::atomic<T>::value_type desired ) noexcept;  // (2) (since C++11)
template< class T >
void atomic_store_explicit( std::atomic<T>* obj,
                            typename std::atomic<T>::value_type desired,
                            std::memory_order order) noexcept;  // (3) (since C++11)
template< class T >
void atomic_store_explicit( volatile std::atomic<T>* obj,
                            typename std::atomic<T>::value_type desired,
                            std::memory_order order) noexcept;  // (4) (since C++11)
```

1,2) Atomically replaces the value pointed to by `obj` with the value of
   `desired` as if by `obj->store(desired)`.

3,4) Atomically replaces the value pointed to by `obj` with the value of
   `desired` as if by `obj->store(desired, order)`.

If `order` is one of `std::memory_order_consume`, `std::memory_order_acquire`
   and `std::memory_order_acq_rel`, the behavior is undefined.

### Parameters

- **obj** — pointer to the atomic object to modify
- **desired** — the value to store in the atomic object
- **order** — the memory synchronization ordering

### Return value

(none)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | exact type match was required because `T` was deduced from
      multiple arguments | `T` is only deduced from `obj`

### See also

- **store** — atomically replaces the value of the atomic object with a
  non-atomic argument (public member function of `std::atomic<T>`)
- **atomic_loadatomic_load_explicit (C++11)(C++11)** — atomically obtains the
  value stored in an atomic object (function template)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

- **std::atomic_store(std::shared_ptr)
  std::atomic_store_explicit(std::shared_ptr) (deprecated in C++20)** —
  specializes atomic operations for `std::shared_ptr` (function template)

**C documentation for `atomic_store, atomic_store_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_store*
