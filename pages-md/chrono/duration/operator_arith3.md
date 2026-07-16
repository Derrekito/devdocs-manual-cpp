# std::chrono::duration<Rep,Period>::operator+=, -=, *=, /=, %=

```cpp
duration& operator+=( const duration& d );  // (since C++11) (constexpr since C++17)
duration& operator-=( const duration& d );  // (since C++11) (constexpr since C++17)
duration& operator*=( const rep& rhs );  // (since C++11) (constexpr since C++17)
duration& operator/=( const rep& rhs );  // (since C++11) (constexpr since C++17)
duration& operator%=( const rep& rhs );  // (since C++11) (constexpr since C++17)
duration& operator%=( const duration& rhs );  // (since C++11) (constexpr since C++17)
```

Performs compound assignments between two durations with the same period or
between a duration and a tick count value.

If `rep_` is the member variable holding the number of ticks in this duration
object,

1) Equivalent to `rep_ += d.count(); return *this;`.

2) Equivalent to `rep_ -= d.count(); return *this;`.

3) Equivalent to `rep_ *= rhs; return *this;`.

4) Equivalent to `rep_ /= rhs; return *this;`.

5) Equivalent to `rep_ %= rhs; return *this;`.

6) Equivalent to `rep_ %= d.count(); return *this;`.

### Parameters

- **d** — duration on the right-hand side of the operator
- **rhs** — number of ticks on the right-hand side of the operator

### Return value

A reference to this duration after modification.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::chrono::minutes m(11);
    m *= 2;
    m += std::chrono::hours(10); // hours implicitly convert to minutes
    std::cout << m.count() << " minutes equals "
              << std::chrono::duration_cast<std::chrono::hours>(m).count()
              << " hours and ";
    m %= std::chrono::hours(1);
    std::cout << m.count() << " minutes\n";
}
```

Output:

```text
622 minutes equals 10 hours and 22 minutes
```

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the tick count (public member function)
- **operator+operator-operator*operator/operator% (C++11)** — implements
  arithmetic operations with durations as arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/operator_arith3*
