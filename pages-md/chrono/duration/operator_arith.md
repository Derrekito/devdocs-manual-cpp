# std::chrono::duration<Rep,Period>::operator+(unary), std::chrono::duration<Rep,Period>::operator-(unary)

```cpp
constexpr duration operator+() const;  // (until C++17)
constexpr std::common_type_t<duration> operator+() const;  // (since C++17)
constexpr duration operator-() const;  // (until C++17)
constexpr std::common_type_t<duration> operator-() const;  // (since C++17)
```

Implements unary plus and unary minus for the durations.

If `rep_` is a member variable holding the number of ticks in a duration object,
and `D` is the return type,

1) Equivalent to `return D(*this);`.

2) Equivalent to `return D(-rep_);`.

### Parameters

(none)

### Return value

1) A copy of this duration object.

2) A copy of this duration object, with the number of ticks negated.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    constexpr std::chrono::seconds s1(-052);
    constexpr std::chrono::seconds s2 = -s1;

    std::cout << "Negated " << s1 << " are " << s2 << '\n';
}
```

Output:

```text
Negated -42s are 42s
```

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the tick count (public member function)
- **operator+operator-operator*operator/operator% (C++11)** — implements
  arithmetic operations with durations as arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/operator_arith*
