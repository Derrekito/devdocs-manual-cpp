# std::clamp

```cpp
template< class T >
constexpr const T& clamp( const T& v, const T& lo, const T& hi );  // (1) (since C++17)
template< class T, class Compare >
constexpr const T& clamp( const T& v, const T& lo, const T& hi, Compare comp );  // (2) (since C++17)
```

1) If `v` compares less than `lo`, returns `lo`; otherwise if `hi` compares less
   than `v`, returns `hi`; otherwise returns `v`.

Uses `operator<` to compare the values.

2) Same as (1), but uses `comp` to compare the values.

The behavior is undefined if the value of `lo` is greater than `hi`.

### Parameters

- **v** — the value to clamp
- **lo,hi** — the boundaries to clamp `v` to
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if the first argument is *less*
  than the second. The signature of the comparison function should be equivalent
  to the following: `bool cmp(const Type1& a, const Type2& b);` While the
  signature does not need to have `const&`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1&` is not
  allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that an object of
  type T can be implicitly converted to both of them.

**Type requirements**

**-`T` must meet the requirements of LessThanComparable in order to use overloads (1). However, if `NaN` is avoided, `T` can be a floating-point type.**

### Return value

Reference to `lo` if `v` is less than `lo`, reference to `hi` if `hi` is less
than `v`, otherwise reference to `v`.

### Complexity

At most two comparisons.

### Possible implementation

```cpp
template<class T>
constexpr const T& clamp(const T& v, const T& lo, const T& hi)
{
    return clamp(v, lo, hi, less{});
}
```

```cpp
template<class T, class Compare>
constexpr const T& clamp(const T& v, const T& lo, const T& hi, Compare comp)
{
    return comp(v, lo) ? lo : comp(hi, v) ? hi : v;
}
```

### Notes

Capturing the result of `std::clamp` by reference produces a dangling reference
if one of the parameters is a temporary and that parameter is returned:

```cpp
int n = -1;
const int& r = std::clamp(n, 0, 255); // r is dangling
```

If `v` compares equivalent to either bound, returns a reference to `v`, not the
bound.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_clamp` | 201603L | (C++17) | `std::clamp`

### Example

```cpp
#include <algorithm>
#include <cstdint>
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << " raw   clamped to int8_t   clamped to uint8_t\n";
    for (const int v : {-129, -128, -1, 0, 42, 127, 128, 255, 256})
    {
        std::cout
            << std::setw(04) << v
            << std::setw(20) << std::clamp(v, INT8_MIN, INT8_MAX)
            << std::setw(21) << std::clamp(v, 0, UINT8_MAX) << '\n';
    }
}
```

Output:

```text
 raw   clamped to int8_t   clamped to uint8_t
-129                -128                    0
-128                -128                    0
  -1                  -1                    0
   0                   0                    0
  42                  42                   42
 127                 127                  127
 128                 127                  128
 255                 127                  255
 256                 127                  255
```

### See also

- **min** — returns the smaller of the given values (function template)
- **max** — returns the greater of the given values (function template)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)
- **ranges::clamp (C++20)** — clamps a value between a pair of boundary values
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/clamp*
