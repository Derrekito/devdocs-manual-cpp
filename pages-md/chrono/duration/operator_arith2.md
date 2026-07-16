# std::chrono::duration<Rep,Period>::operator++, std::chrono::duration<Rep,Period>::operator--

```cpp
duration& operator++();  // (1) (since C++11) (constexpr since C++17)
duration operator++( int );  // (2) (since C++11) (constexpr since C++17)
duration& operator--();  // (3) (since C++11) (constexpr since C++17)
duration operator--( int );  // (4) (since C++11) (constexpr since C++17)
```

Increments or decrements the number of ticks for this duration.

If `rep_` is a member variable holding the number of ticks in a duration object,

1) Equivalent to `++rep_; return *this;`.

2) Equivalent to `return duration(rep_++)`.

3) Equivalent to `--rep_; return *this;`.

4) Equivalent to `return duration(rep_--);`.

### Parameters

(none)

### Return value

1,3) A reference to this duration after modification.

2,4) A copy of the duration made before modification.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::chrono::hours h(1);
    std::chrono::minutes m = ++h;
    m--;
    std::cout << m.count() << " minutes\n";
}
```

Output:

```text
119 minutes
```

### See also

- **operator+=operator-=operator*=operator/=operator%=** — implements compound
  assignment between two durations (public member function)
- **operator+operator-operator*operator/operator% (C++11)** — implements
  arithmetic operations with durations as arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/operator_arith2*
