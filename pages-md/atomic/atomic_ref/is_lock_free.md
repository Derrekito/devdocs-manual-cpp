# std::atomic_ref<T>::is_lock_free

```cpp
bool is_lock_free() const noexcept;  // (since C++20)
```

Checks whether the atomic operations on this object are lock-free.

### Parameters

(none)

### Return value

`true` if the atomic operations on this object are lock-free, `false` otherwise.

### Notes

All atomic types except for `std::atomic_flag` may be implemented using mutexes
or other locking operations, rather than using the lock-free atomic CPU
instructions. Atomic types are also allowed to be *sometimes* lock-free, e.g. if
only aligned memory accesses are naturally atomic on a given architecture,
misaligned objects of the same type have to use locks.

The C++ standard recommends (but does not require) that lock-free atomic
operations are also address-free, that is, suitable for communication between
processes using shared memory.

### Example

### See also

- **is_always_lock_free [static]** — indicates that the type is always lock-free
  (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/is_lock_free*
