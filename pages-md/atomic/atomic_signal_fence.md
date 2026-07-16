# std::atomic_signal_fence

```cpp
extern "C" void atomic_signal_fence( std::memory_order order ) noexcept;  // (since C++11)
```

Establishes memory synchronization ordering of non-atomic and relaxed atomic
accesses, as instructed by `order`, between a thread and a signal handler
executed on the same thread. This is equivalent to `std::atomic_thread_fence`,
except no CPU instructions for memory ordering are issued. Only reordering of
the instructions by the compiler is suppressed as `order` instructs. For
example, a fence with release semantics prevents reads or writes from being
moved past subsequent writes and a fence with acquire semantics prevents reads
or writes from being moved ahead of preceding reads.

### Parameters

- **order** — the memory ordering executed by this fence

### Return value

(none)

### Example

### See also

- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)
- **atomic_thread_fence (C++11)** — generic memory order-dependent fence
  synchronization primitive (function)

**C documentation for `atomic_signal_fence`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_signal_fence*
