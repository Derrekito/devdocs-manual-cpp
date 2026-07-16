# std::atomic<T>::fetch_sub

```cpp
member only of atomic<Integral ﻿> specializationsand atomic<Floating ﻿> specializations(since C++20)
T fetch_sub( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) noexcept;  // (1) (since C++11)
T fetch_sub( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) volatile noexcept;  // (2) (since C++11)
member only of atomic<T*> partial specialization
T* fetch_sub( std::ptrdiff_t arg,
              std::memory_order order =
                  std::memory_order_seq_cst ) noexcept;  // (3) (since C++11)
T* fetch_sub( std::ptrdiff_t arg,
              std::memory_order order =
                  std::memory_order_seq_cst ) volatile noexcept;  // (4) (since C++11)
```

Atomically replaces the current value with the result of arithmetic subtraction
of the value and `arg`. That is, it performs atomic post-decrement. The
operation is read-modify-write operation. Memory is affected according to the
value of `order`.

1,2) For signed integral types, arithmetic is defined to use two’s complement
   representation. There are no undefined results. For floating-point types, the
   floating-point environment in effect may be different from the calling
   thread's floating-point environment. The operation need not be conform to the
   corresponding `std::numeric_limits` traits but is encouraged to do so. If the
   result is not a representable value for its type, the result is unspecified
   but the operation otherwise has no undefined behavior. (since C++20)

3,4) The result may be an undefined address, but the operation otherwise has no
   undefined behavior.

If `T` is not a complete object type, the program is ill-formed.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) or (4) participates in overload resolution.
*(since C++20)*

### Parameters

- **arg** — the other argument of arithmetic subtraction
- **order** — memory order constraints to enforce

### Return value

The value immediately preceding the effects of this function in the modification
order of `*this`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | arithmetic permitted on pointers to (possibly cv-qualified)
      void or function | made ill-formed

### See also

- **atomic_fetch_subatomic_fetch_sub_explicit (C++11)(C++11)** — subtracts a
  non-atomic value from an atomic object and obtains the previous value of the
  atomic (function template)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the atomic value by one (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/fetch_sub*
