# std::max

```cpp
template< class T >
const T& max( const T& a, const T& b );  // (1) (constexpr since C++14)
template< class T, class Compare >
const T& max( const T& a, const T& b, Compare comp );  // (2) (constexpr since C++14)
template< class T >
T max( std::initializer_list<T> ilist );  // (3) (since C++11) (constexpr since C++14)
template< class T, class Compare >
T max( std::initializer_list<T> ilist, Compare comp );  // (4) (since C++11) (constexpr since C++14)
```

Returns the greater of the given values.

1,2) Returns the greater of `a` and `b`.

3,4) Returns the greatest of the values in initializer list `ilist`.

Overloads (1,3) use `operator<` to compare the values, overloads (2,4) use the
given comparison function `comp`.

### Parameters

- **a, b** — the values to compare
- **ilist** — initializer list with the values to compare
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if `a` is *less* than `b`. The
  signature of the comparison function should be equivalent to the following:
  `bool cmp(const Type1& a, const Type2& b);` While the signature does not need
  to have `const&`, the function must not modify the objects passed to it and
  must be able to accept all values of type (possibly const) `Type1` and `Type2`
  regardless of value category (thus, `Type1&` is not allowed, nor is `Type1`
  unless for `Type1` a move is equivalent to a copy(since C++11)). The types
  Type1 and Type2 must be such that an object of type T can be implicitly
  converted to both of them.

**Type requirements**

**-`T` must meet the requirements of LessThanComparable in order to use overloads (1,3).**

**-`T` must meet the requirements of CopyConstructible in order to use overloads (3,4).**

### Return value

1,2) The greater of `a` and `b`. If they are equivalent, returns `a`.

3,4) The greatest value in `ilist`. If several values are equivalent to the
   greatest, returns the leftmost one.

### Complexity

1,2) Exactly one comparison.

3,4) Exactly `ilist.size() - 1` comparisons.

### Possible implementation

```cpp
template<class T>
const T& max(const T& a, const T& b)
{
    return (a < b) ? b : a;
}
```

```cpp
template<class T, class Compare>
const T& max(const T& a, const T& b, Compare comp)
{
    return (comp(a, b)) ? b : a;
}
```

```cpp
template<class T>
T max(std::initializer_list<T> ilist)
{
    return *std::max_element(ilist.begin(), ilist.end());
}
```

```cpp
template<class T, class Compare>
T max(std::initializer_list<T> ilist, Compare comp)
{
    return *std::max_element(ilist.begin(), ilist.end(), comp);
}
```

### Notes

Capturing the result of `std::max` by reference produces a dangling reference if
one of the parameters is a temporary and that parameter is returned:

```cpp
int n = -1;
const int& r = std::max(n + 2, n * 2); // r is dangling
```

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <string_view>

int main()
{
    auto longest = [](const std::string_view s1, const std::string_view s2)
                   {
                       return s1.size() < s2.size();
                   };

    std::cout << "Larger of 69 and 96 is " << std::max(69, 96) << "\n"
                 "Larger of 'q' and 'p' is '" << std::max('q', 'p') << "'\n"
                 "Largest of 010, 10, 0X10, and 0B10 is "
              << std::max({010, 10, 0X10, 0B10}) << '\n'
              << R"(Longest of "long", "short", and "int" is )"
              << std::quoted(std::max({"long", "short", "int"}, longest)) << '\n';
}
```

Output:

```text
Larger of 69 and 96 is 96
Larger of 'q' and 'p' is 'q'
Largest of 010, 10, 0X10, and 0B10 is 16
Longest of "long", "short", and "int" is "short"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 281 | C++98 | `T` was required to be CopyConstructible for overloads (1,2)
      | not required

### See also

- **min** — returns the smaller of the given values (function template)
- **minmax (C++11)** — returns the smaller and larger of two elements (function
  template)
- **max_element** — returns the largest element in a range (function template)
- **clamp (C++17)** — clamps a value between a pair of boundary values (function
  template)
- **ranges::max (C++20)** — returns the greater of the given values (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/max*
