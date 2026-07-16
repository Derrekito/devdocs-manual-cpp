# std::compare_weak_order_fallback

```cpp
inline namespace /* unspecified */ {
    inline constexpr /* unspecified */
        compare_weak_order_fallback = /* unspecified */;
}  // (since C++20)
Call signature
template< class T, class U >
    requires /* see below */
constexpr std::weak_ordering
    compare_weak_order_fallback( T&& t, U&& u ) noexcept(/* see below */);  // (since C++20)
```

Performs three-way comparison on subexpressions `t` and `u` and produces a
result of type `std::weak_ordering`, even if the operator `<=>` is unavailable.

If std::decay_t<T> and std::decay_t<U> are the same type,
`std::compare_weak_order_fallback(t, u)` is expression-equivalent to:

- `std::weak_order(t, u)`, if it is a well-formed expression;
- otherwise, `t == u ? std::weak_ordering::equivalent :t < u ?
  std::weak_ordering::less : std::weak_ordering::greater`,

if the expressions `t == u` and `t < u` are both well-formed and each of
decltype(t == u) and decltype(t < u) models `boolean-testable`, except that `t`
and `u` are evaluated only once.

In all other cases, `std::compare_weak_order_fallback(t, u)` is ill-formed,
which can result in substitution failure when it appears in the immediate
context of a template instantiation.

### Customization point objects

The name `std::compare_weak_order_fallback` denotes a *customization point
object*, which is a const function object of a literal `semiregular` class type.
For exposition purposes, the cv-unqualified version of its type is denoted as
`__compare_weak_order_fallback_fn`.

All instances of `__compare_weak_order_fallback_fn` are equal. The effects of
invoking different instances of type `__compare_weak_order_fallback_fn` on the
same arguments are equivalent, regardless of whether the expression denoting the
instance is an lvalue or rvalue, and is const-qualified or not (however, a
volatile-qualified instance is not required to be invocable). Thus,
`std::compare_weak_order_fallback` can be copied freely and its copies can be
used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `std::compare_weak_order_fallback` above,
`__compare_weak_order_fallback_fn` models

- `std::invocable<__compare_weak_order_fallback_fn, Args...>`,
- `std::invocable<const __compare_weak_order_fallback_fn, Args...>`,
- `std::invocable<__compare_weak_order_fallback_fn&, Args...>`, and
- `std::invocable<const __compare_weak_order_fallback_fn&, Args...>`.

Otherwise, no function call operator of `__compare_weak_order_fallback_fn`
participates in overload resolution.

### Example

```cpp
#include <compare>
#include <iostream>

// does not support <=>
struct Rational_1
{
    int num;
    int den; // > 0
};

inline constexpr bool operator<(Rational_1 lhs, Rational_1 rhs)
{
    return lhs.num * rhs.den < rhs.num * lhs.den;
}

inline constexpr bool operator==(Rational_1 lhs, Rational_1 rhs)
{
    return lhs.num * rhs.den == rhs.num * lhs.den;
}

// supports <=>
struct Rational_2
{
    int num;
    int den; // > 0
};

inline constexpr std::weak_ordering operator<=>(Rational_2 lhs, Rational_2 rhs)
{
    return lhs.num * rhs.den <=> rhs.num * lhs.den;
}

inline constexpr bool operator==(Rational_2 lhs, Rational_2 rhs)
{
    return lhs <=> rhs == 0;
}

void print(int id, std::weak_ordering value)
{
    std::cout << id << ") ";
    if (value == 0)
        std::cout << "equal\n";
    else if (value < 0)
        std::cout << "less\n";
    else
        std::cout << "greater\n";
}

int main()
{
    Rational_1 a{1, 2}, b{3, 4};
//  print(1, a <=> b); // does not work
    print(2, std::compare_weak_order_fallback(a, b)); // works, defaults to < and ==

    Rational_2 c{6, 5}, d{8, 7};
    print(3, c <=> d); // works
    print(4, std::compare_weak_order_fallback(c, d)); // works

    Rational_2 e{2, 3}, f{4, 6};
    print(5, e <=> f); // works
    print(6, std::compare_weak_order_fallback(e, f)); // works
}
```

Output:

```text
2) less
3) greater
4) greater
5) equal
6) equal
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2114 (P2167R3) | C++20 | the fallback mechanism only required return types
      to be convertible to bool | constraints strengthened

### See also

- **weak_order (C++20)** — performs 3-way comparison and produces a result of
  type `std::weak_ordering` (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/utility/compare/compare_weak_order_fallback*
