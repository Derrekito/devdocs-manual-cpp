# std::atomic_notify_all

```cpp
template< class T >
void atomic_notify_all( std::atomic<T>* object );  // (1) (since C++20)
template< class T >
void atomic_notify_all( volatile std::atomic<T>* object );  // (2) (since C++20)
```

Performs atomic notifying operations.

Unblocks all threads blocked in atomic waiting operations (i.e.
`std::atomic_wait()`, `std::atomic_wait_explicit()`, or `std::atomic::wait()`)
on `*object`, if there are any; otherwise does nothing.

Equivalent to `object->notify_all()`.

### Parameters

- **object** — pointer to the atomic object to notify

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

### Example

### See also

- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function of `std::atomic<T>`)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function of `std::atomic<T>`)
- **atomic_waitatomic_wait_explicit (C++20)(C++20)** — blocks the thread until
  notified and the atomic value changes (function template)
- **atomic_notify_one (C++20)** — notifies a thread blocked in atomic_wait
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_notify_all*
