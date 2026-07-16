# std::compare_three_way

```cpp
struct compare_three_way;  // (since C++20)
```

Function object for performing comparisons. Deduces the parameter types and the
return type of the function call operator.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- **operator()** — obtains the result of three-way comparison on both arguments
  (public member function)

## std::compare_three_way::operator()

```cpp
template< class T, class U >
constexpr auto operator()( T&& t, U&& u ) const;
```

Given the expression `std::forward<T>(t) <=> std::forward<U>(u)` as `expr`:

- If `expr` results in a call to built-in operator<=> comparing pointers, given
  the composite pointer type of `t` and `u` as `P`:
- Compares the two converted pointers (of type `P`) in the
  implementation-defined strict total order over pointers:
- If the conversion sequence from `T` to `P` or the conversion sequence from `U`
  to `P` is not equality-preserving, the behavior is undefined.
- Otherwise:

This overload participates in overload resolution only if
`std::three_way_comparable_with<T, U>` is satisfied.

### Example

```cpp
#include <compare>
#include <iostream>

struct Rational
{
    int num;
    int den; // > 0

    // Although the comparison X <=> Y will work, a direct call
    // to std::compare_three_way{}(X, Y) requires the operator==
    // be defined, to satisfy the std::three_way_comparable_with.
    constexpr bool operator==(Rational const&) const = default;
};

constexpr std::weak_ordering operator<=>(Rational lhs, Rational rhs)
{
    return lhs.num * rhs.den <=> rhs.num * lhs.den;
}

void print(std::weak_ordering value)
{
    value < 0 ? std::cout << "less\n" :
    value > 0 ? std::cout << "greater\n" :
                std::cout << "equal\n";
}

int main()
{
    Rational a{6, 5};
    Rational b{8, 7};
    print(a <=> b);
    print(std::compare_three_way{}(a, b));
}
```

Output:

```text
greater
greater
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3530 | C++20 | syntactic checks were relaxed while comparing pointers |
      only semantic requirements are relaxed

### See also

- **ranges::equal_to (C++20)** — function object implementing `x == y` (class)
- **ranges::not_equal_to (C++20)** — function object implementing `x != y`
  (class)
- **ranges::less (C++20)** — function object implementing `x < y` (class)
- **ranges::greater (C++20)** — function object implementing `x > y` (class)
- **ranges::less_equal (C++20)** — function object implementing `x <= y` (class)
- **ranges::greater_equal (C++20)** — function object implementing `x >= y`
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/compare_three_way*
