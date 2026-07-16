# std::atomic<T>::operator++,++(int),--,--(int)

```cpp
member only of atomic<Integral ﻿> specializations
T operator++() noexcept;  // (1) (since C++11)
T operator++() volatile noexcept;  // (2) (since C++11)
T operator++( int ) noexcept;  // (3) (since C++11)
T operator++( int ) volatile noexcept;  // (4) (since C++11)
T operator--() noexcept;  // (5) (since C++11)
T operator--() volatile noexcept;  // (6) (since C++11)
T operator--( int ) noexcept;  // (7) (since C++11)
T operator--( int ) volatile noexcept;  // (8) (since C++11)
member only of atomic<T*> partial specialization
T* operator++() noexcept;  // (9) (since C++11)
T* operator++() volatile noexcept;  // (10) (since C++11)
T* operator++( int ) noexcept;  // (11) (since C++11)
T* operator++( int ) volatile noexcept;  // (12) (since C++11)
T* operator--() noexcept;  // (13) (since C++11)
T* operator--() volatile noexcept;  // (14) (since C++11)
T* operator--( int ) noexcept;  // (15) (since C++11)
T* operator--( int ) volatile noexcept;  // (16) (since C++11)
```

Atomically increments or decrements the current value. The operation is
read-modify-write operation.

- operator++() performs atomic pre-increment. Equivalent to `return fetch_add(1)
  + 1;`.
- operator++(int) performs atomic post-increment. Equivalent to `return
  fetch_add(1);`.
- operator--() performs atomic pre-decrement. Equivalent to `return fetch_sub(1)
  - 1;`.
- operator--(int) performs atomic post-decrement. Equivalent to `return
  fetch_sub(1);`.

1-8) For signed integral types, arithmetic is defined to use two’s complement
   representation. There are no undefined results.

9-16) The result may be an undefined address, but the operations otherwise have
   no undefined behavior.

If `T` is not a complete object type, the program is ill-formed.

It is deprecated if `std::atomic<T>::is_always_lock_free` is false and any
volatile overload participates in overload resolution.
*(since C++20)*

### Parameters

(none)

### Return value

operator++() and operator--() return the value of the atomic variable after the
modification. Formally, the result of incrementing/decrementing the value
immediately preceding the effects of this function in the modification order of
`*this`.

operator++(int) and operator--(int) return the value of the atomic variable
before the modification. Formally, the value immediately preceding the effects
of this function in the modification order of `*this`.

### Notes

Unlike most pre-increment and pre-decrement operators, the pre-increment and
pre-decrement operators for atomic types do not return a reference to the
modified object. They return a copy of the stored value instead.

### Example

```cpp
#include <atomic>
#include <chrono>
#include <iomanip>
#include <iostream>
#include <mutex>
#include <random>
#include <string>
#include <thread>

std::atomic<int> atomic_count{0};

std::mutex cout_mutex;
int completed_writes{0};

constexpr int global_max_count{72};
constexpr int writes_per_line{8};
constexpr int max_delay{100};

template<int Max>
int random_value()
{
    static std::uniform_int_distribution<int> distr{1, Max};
    static std::random_device engine;
    static std::mt19937 noise{engine()};
    static std::mutex rand_mutex;
    std::lock_guard lock{rand_mutex};
    return distr(noise);
}

int main()
{
    auto work = [](const std::string id)
    {
        for (int count{}; (count = ++atomic_count) <= global_max_count;)
        {
            std::this_thread::sleep_for(
                std::chrono::milliseconds(random_value<max_delay>()));

            // print thread `id` and `count` value
            {
                std::lock_guard lock{cout_mutex};

                const bool new_line = ++completed_writes % writes_per_line == 0;

                std::cout << id << std::setw(3) << count << "  "
                          << (new_line ? "\n" : "") << std::flush;
            }
        }
    };

    std::jthread j1(work, "░"), j2(work, "▒"), j3(work, "▓"), j4(work, "█");
}
```

Possible output:

```text
▒  2  ░  1  ▒  5  ▒  7  █  4  ░  6  ▓  3  ▒  8
▓ 11  █  9  ▓ 13  ░ 10  █ 14  ▒ 12  ░ 16  ░ 19
▓ 15  ▒ 18  ▓ 21  ▒ 22  █ 17  █ 25  ▒ 24  █ 26
░ 20  ░ 29  ▒ 27  ▓ 23  ▒ 31  ▒ 33  ▓ 32  █ 28
░ 30  ░ 37  ▒ 34  ▓ 35  █ 36  █ 41  ▓ 40  ▒ 39
░ 38  ▓ 43  █ 42  ▓ 46  ▓ 48  █ 47  █ 50  ░ 45
▒ 44  ▒ 53  ▒ 54  ▓ 49  ▒ 55  █ 51  ▒ 57  █ 58
░ 52  ▓ 56  ░ 61  ▒ 59  █ 60  ▓ 62  ▒ 64  ░ 63
░ 68  ▓ 66  █ 65  █ 71  ▒ 67  ▓ 70  ░ 69  █ 72
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | arithmetic permitted on pointers to (possibly cv-qualified)
      void or function | made ill-formed

### See also

- **fetch_add** — atomically adds the argument to the value stored in the atomic
  object and obtains the value held previously (public member function)
- **fetch_sub** — atomically subtracts the argument from the value stored in the
  atomic object and obtains the value held previously (public member function)
- **operator+=operator-=operator&=operator|=operator^=** — adds, subtracts, or
  performs bitwise AND, OR, XOR with the atomic value (public member function)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/operator_arith*
