# std::atomic<T>::fetch_add

```cpp
member only of atomic<Integral ﻿> specializationsand atomic<Floating ﻿> specializations(since C++20)
T fetch_add( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) noexcept;  // (1) (since C++11)
T fetch_add( T arg, std::memory_order order =
                        std::memory_order_seq_cst ) volatile noexcept;  // (2) (since C++11)
member only of atomic<T*> partial specialization
T* fetch_add( std::ptrdiff_t arg,
              std::memory_order order =
                  std::memory_order_seq_cst ) noexcept;  // (3) (since C++11)
T* fetch_add( std::ptrdiff_t arg,
              std::memory_order order =
                  std::memory_order_seq_cst ) volatile noexcept;  // (4) (since C++11)
```

Atomically replaces the current value with the result of arithmetic addition of
the value and `arg`. That is, it performs atomic post-increment. The operation
is a read-modify-write operation. Memory is affected according to the value of
`order`.

1,2) For signed integral types, arithmetic is defined to use two’s complement
   representation. There are no undefined results. For floating-point types, the
   floating-point environment in effect may be different from the calling
   thread's floating-point environment. The operation need not conform to the
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

- **arg** — the other argument of arithmetic addition
- **order** — memory order constraints to enforce

### Return value

The value immediately preceding the effects of this function in the modification
order of `*this`.

### Example

```cpp
#include <array>
#include <atomic>
#include <iostream>
#include <thread>

std::atomic<long long> data{10};
std::array<long long, 5> return_values{};

void do_work(int thread_num)
{
    long long val = data.fetch_add(1, std::memory_order_relaxed);
    return_values[thread_num] = val;
}

int main()
{
    {
        std::jthread th0{do_work, 0};
        std::jthread th1{do_work, 1};
        std::jthread th2{do_work, 2};
        std::jthread th3{do_work, 3};
        std::jthread th4{do_work, 4};
    }

    std::cout << "Result : " << data << '\n';

    for (long long val : return_values)
        std::cout << "Seen return value : " << val << std::endl;
}
```

Possible output:

```text
Result : 15
Seen return value : 11
Seen return value : 10
Seen return value : 14
Seen return value : 12
Seen return value : 13
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | arithmetic permitted on pointers to (possibly cv-qualified)
      void or function | made ill-formed

### See also

- **atomic_fetch_addatomic_fetch_add_explicit (C++11)(C++11)** — adds a
  non-atomic value to an atomic object and obtains the previous value of the
  atomic (function template)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the atomic value by one (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/fetch_add*
