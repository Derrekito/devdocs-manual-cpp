# std::chrono::operator==,<=>(std::chrono::month)

```cpp
constexpr bool operator==( const std::chrono::month& x,
                           const std::chrono::month& y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering operator<=>( const std::chrono::month& x,
                                            const std::chrono::month& y ) noexcept;  // (2) (since C++20)
```

Compare the two `std::chrono::month` `x` and `y`.

The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.

### Return value

1) `unsigned(x) == unsigned(y)`

2) `unsigned(x) <=> unsigned(y)`

### Example

```cpp
#include <chrono>

int main()
{
    constexpr std::chrono::month m1{6}, m2{8};

    static_assert
    (
        m1 < m2 && m1 == m1 && m1 <= m2 &&
        m2 > m1 && m2 != m1 && m2 >= m1 &&
        m1 <=> m2 != 0
    );
}
```

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/operator_cmp*
