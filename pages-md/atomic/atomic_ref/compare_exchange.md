# std::atomic_ref<T>::compare_exchange_weak, std::atomic_ref<T>::compare_exchange_strong

```cpp
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order success,
                            std::memory_order failure ) const noexcept;  // (1) (since C++20)
bool compare_exchange_weak( T& expected, T desired,
                            std::memory_order order =
                                std::memory_order_seq_cst ) const noexcept;  // (2) (since C++20)
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order success,
                              std::memory_order failure ) const noexcept;  // (3) (since C++20)
bool compare_exchange_strong( T& expected, T desired,
                              std::memory_order order =
                                  std::memory_order_seq_cst ) const noexcept;  // (4) (since C++20)
```

Atomically compares the value representation of the referenced object with that
of `expected`, and if those are bitwise-equal, replaces the former with
`desired` (performs a read-modify-write operation). Otherwise, loads the actual
value stored in the referenced object into `expected` (performs a load
operation).

The memory models for the read-modify-write and load operations are `success`
and `failure` respectively. In the (2) and (4) versions `order` is used for both
read-modify-write and load operations, except that `std::memory_order_acquire`
and `std::memory_order_relaxed` are used for the load operation if `order ==
std::memory_order_acq_rel`, or `order == std::memory_order_release`
respectively.

### Parameters

- **expected** — reference to the value expected to be found in the object
  referenced by the `atomic_ref` object
- **desired** — the value to store in the referenced object if it is as expected
- **success** — the memory synchronization ordering for the read-modify-write
  operation if the comparison succeeds. All values are permitted
- **failure** — the memory synchronization ordering for the load operation if
  the comparison fails. Cannot be `std::memory_order_release` or
  `std::memory_order_acq_rel`
- **order** — the memory synchronization ordering for both operations

### Return value

`true` if the referenced object was successfully changed, `false` otherwise.

### Notes

The comparison and copying are bitwise (similar to `std::memcmp` and
`std::memcpy`); no constructor, assignment operator, or comparison operator are
used.

The weak forms (1,2) of the functions are allowed to fail spuriously, that is,
act as if `*this != expected` even if they are equal. When a
compare-and-exchange is in a loop, the weak version will yield better
performance on some platforms.

When a weak compare-and-exchange would require a loop and a strong one would
not, the strong one is preferable unless the object representation of `T` may
include trap bits, or offers multiple object representations for the same value
(e.g. floating-point NaN). In those cases, weak compare-and-exchange typically
works because it quickly converges on some stable object representation.

For a union with bits that participate in the value representations of some
members but not the others, compare-and-exchange might always fail because such
padding bits have indeterminate values when they do not participate in the value
representation of the active member.

Padding bits that never participate in an object's value representation are
ignored.

### Example

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_ref/compare_exchange*
