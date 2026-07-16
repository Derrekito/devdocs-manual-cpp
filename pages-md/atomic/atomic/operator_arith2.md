# std::atomic<T>::operator+=,-=,&=,|=,^=

```cpp
member only of atomic<Integral ﻿> specializationsand atomic<Floating ﻿> specializations(since C++20)
T operator+=( T arg ) noexcept;  // (1) (since C++11)
T operator+=( T arg ) volatile noexcept;  // (2) (since C++11)
T operator-=( T arg ) noexcept;  // (3) (since C++11)
T operator-=( T arg ) volatile noexcept;  // (4) (since C++11)
member only of atomic<T*> partial specialization
T* operator+=( std::ptrdiff_t arg ) noexcept;  // (5) (since C++11)
T* operator+=( std::ptrdiff_t arg ) volatile noexcept;  // (6) (since C++11)
T* operator-=( std::ptrdiff_t arg ) noexcept;  // (7) (since C++11)
T* operator-=( std::ptrdiff_t arg ) volatile noexcept;  // (8) (since C++11)
member only of atomic<Integral ﻿> specializations
T operator&=( T arg ) noexcept;  // (9) (since C++11)
T operator&=( T arg ) volatile noexcept;  // (10) (since C++11)
T operator|=( T arg ) noexcept;  // (11) (since C++11)
T operator|=( T arg ) volatile noexcept;  // (12) (since C++11)
T operator^=( T arg ) noexcept;  // (13) (since C++11)
T operator^=( T arg ) volatile noexcept;  // (14) (since C++11)
```

Atomically replaces the current value with the result of computation involving
the previous value and `arg`. The operation is read-modify-write operation.

- operator+= performs atomic addition. Equivalent to `return fetch_add(arg) +
  arg;`.
- operator-= performs atomic subtraction. Equivalent to `return fetch_sub(arg) -
  arg;`.
- operator&= performs atomic bitwise and. Equivalent to `return fetch_and(arg) &
  arg;`.
- operator|= performs atomic bitwise or. Equivalent to `return fetch_or(arg) |
  arg;`.
- operator^= performs atomic bitwise exclusive or. Equivalent to `return
  fetch_xor(arg) ^ arg;`.

1-4) For signed integral types, arithmetic is defined to use two’s complement
   representation. There are no undefined results. For floating-point types, the
   floating-point environment in effect may be different from the calling
   thread's floating-point environment. The operation need not be conform to the
   corresponding `std::numeric_limits` traits but is encouraged to do so. If the
   result is not a representable value for its type, the result is unspecified
   but the operation otherwise has no undefined behavior. (since C++20)

5-8) The result may be an undefined address, but the operations otherwise have
   no undefined behavior.

If `T` is not a complete object type, the program is ill-formed.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and any
volatile overload participates in overload resolution.
*(since C++20)*

### Parameters

- **arg** — the argument for the arithmetic operation

### Return value

The resulting value (that is, the result of applying the corresponding binary
operator to the value immediately preceding the effects of the corresponding
member function in the modification order of `*this`).

### Notes

Unlike most compound assignment operators, the compound assignment operators for
atomic types do not return a reference to their left-hand arguments. They return
a copy of the stored value instead.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | arithmetic permitted on pointers to (possibly cv-qualified)
      void or function | made ill-formed

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the atomic value by one (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/operator_arith2*
