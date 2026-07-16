# std::bitset<N>::all, std::bitset<N>::any, std::bitset<N>::none

```cpp
bool all() const;  // (until C++11)
bool all() const noexcept;  // (since C++11) (constexpr since C++23)
bool any() const;  // (until C++11)
bool any() const noexcept;  // (since C++11) (constexpr since C++23)
bool none() const;  // (until C++11)
bool none() const noexcept;  // (since C++11) (constexpr since C++23)
```

1) Checks if all bits are set to `true`.

2) Checks if any bits are set to `true`.

3) Checks if none of the bits are set to `true`.

### Parameters

(none)

### Return value

1) `true` if all bits are set to `true`, otherwise `false`.

2) `true` if any of the bits are set to `true`, otherwise `false`.

3) `true` if none of the bits are set to `true`, otherwise `false`.

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<4> b1("0000");
    std::bitset<4> b2("0101");
    std::bitset<4> b3("1111");

    std::cout
        << "bitset\t" << "all\t" << "any\t" << "none\n"
        << b1 << '\t' << b1.all() << '\t' << b1.any() << '\t' << b1.none() << '\n'
        << b2 << '\t' << b2.all() << '\t' << b2.any() << '\t' << b2.none() << '\n'
        << b3 << '\t' << b3.all() << '\t' << b3.any() << '\t' << b3.none() << '\n';
}
```

Output:

```text
bitset  all any none
0000    0   0   1
0101    0   1   0
1111    1   1   0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 693 | C++98 | the member function `all()` was not provided | provided

### See also

- **count** — returns the number of bits set to `true` (public member function)
- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/all_any_none*
