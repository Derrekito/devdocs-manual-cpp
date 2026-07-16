# std::atomic<T>::fetch_and

```cpp
member only of atomic<Integral ﻿> specializations
T fetch_and( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) noexcept;  // (1) (since C++11)
T fetch_and( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) volatile noexcept;  // (2) (since C++11)
```

Atomically replaces the current value with the result of bitwise AND of the
value and `arg`. The operation is read-modify-write operation. Memory is
affected according to the value of `order`.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) participates in overload resolution.
*(since C++20)*

### Parameters

- **arg** — the other argument of bitwise AND
- **order** — memory order constraints to enforce

### Return value

The value immediately preceding the effects of this function in the modification
order of `*this`.

### See also

- **atomic_fetch_andatomic_fetch_and_explicit (C++11)(C++11)** — replaces the
  atomic object with the result of bitwise AND with a non-atomic argument and
  obtains the previous value of the atomic (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/fetch_and*
