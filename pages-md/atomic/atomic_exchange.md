# std::atomic_exchange, std::atomic_exchange_explicit

```cpp
template< class T >
T atomic_exchange( std::atomic<T>* obj,
                   typename std::atomic<T>::value_type desired ) noexcept;  // (1) (since C++11)
template< class T >
T atomic_exchange( volatile std::atomic<T>* obj,
                   typename std::atomic<T>::value_type desired ) noexcept;  // (3) (since C++11)
template< class T >
T atomic_exchange_explicit( std::atomic<T>* obj,
                            typename std::atomic<T>::value_type desired,
                            std::memory_order order ) noexcept;  // (3) (since C++11)
template< class T >
T atomic_exchange_explicit( volatile std::atomic<T>* obj,
                            typename std::atomic<T>::value_type desired,
                            std::memory_order order ) noexcept;  // (4) (since C++11)
```

1,2) Atomically replaces the value pointed to by `obj` with the value of
   `desired` and returns the value `obj` held previously, as if by
   `obj->exchange(desired)`.

3,4) Atomically replaces the value pointed to by `obj` with the value of
   `desired` and returns the value `obj` held previously, as if by
   `obj->exchange(desired, order)`.

### Parameters

- **obj** — pointer to the atomic object to modify
- **desired** — the value to store in the atomic object
- **order** — the memory synchronization ordering

### Return value

The value held previously by the atomic object pointed to by `obj`.

### Example

A spinlock mutex can be implemented in userspace using an atomic exchange
operation, similar to `std::atomic_flag_test_and_set`:

```cpp
#include <atomic>
#include <iostream>
#include <thread>
#include <vector>

std::atomic<bool> lock(false); // holds true when locked
                               // holds false when unlocked

int new_line{1}; // the access is synchronized via atomic lock variable

void f(int n)
{
    for (int cnt = 0; cnt < 100; ++cnt)
    {
        while (std::atomic_exchange_explicit(&lock, true, std::memory_order_acquire))
            ; // spin until acquired
        std::cout << n << (new_line++ % 80 ? "" : "\n");
        std::atomic_store_explicit(&lock, false, std::memory_order_release);
    }
}

int main()
{
    std::vector<std::thread> v;
    for (int n = 0; n < 8; ++n)
        v.emplace_back(f, n);
    for (auto& t : v)
        t.join();
}
```

Possible output:

```text
02222222222222222222222002222222222222222222222222222222222222222222222222222222
22222222200022222222202222211111111111110000011111111100000000000000110001111111
00011111000001111110000011111100000111000000001111111111111110000010000001001111
11011111111011111011000000000000111100000000000001111000011133333333333333333333
33333333333333333333333333333333333333333333333333333333333333333333333333333333
44444444444444444444444444444444444444444444444444444444444444444444444444444444
44444444444444444444555555555555555555555555555555555555555555555555555555555555
55555555555555555555555555555555555555556666666666666666666666666666666666666666
66666666666666666666666666666666666666666666666666666666666677777777777777777777
77777777777777777777777777777777777777777777777777777777777777777777777777777777
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | exact type match was required because `T` was deduced from
      multiple arguments | `T` is only deduced from `obj`

### See also

- **exchange** — atomically replaces the value of the atomic object and obtains
  the value held previously (public member function of `std::atomic<T>`)
-
  **atomic_compare_exchange_weakatomic_compare_exchange_weak_explicitatomic_compare_exchange_strongatomic_compare_exchange_strong_explicit
  (C++11)(C++11)(C++11)(C++11)** — atomically compares the value of the atomic
  object with non-atomic argument and performs atomic exchange if equal or
  atomic load if not (function template)

- **std::atomic_exchange(std::shared_ptr)
  std::atomic_exchange_explicit(std::shared_ptr) (deprecated in C++20)** —
  specializes atomic operations for `std::shared_ptr` (function template)

**C documentation for `atomic_exchange, atomic_exchange_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_exchange*
