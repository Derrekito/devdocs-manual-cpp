# decltype specifier

Inspects the declared type of an entity or the type and value category of an
expression.

### Syntax

```text
decltype ( entity )    (1)    (since C++11)
decltype ( expression )    (2)    (since C++11)
```

### Explanation

1) If the argument is an unparenthesized id-expression or an unparenthesized
   class member access expression, then decltype yields the type of the entity
   named by this expression. If there is no such entity, or if the argument
   names a set of overloaded functions, the program is ill-formed. If the
   argument is an unparenthesized id-expression naming a structured binding,
   then decltype yields the *referenced type* (described in the specification of
   the structured binding declaration). (since C++17) If the argument is an
   unparenthesized id-expression naming a non-type template parameter, then
   decltype yields the type of the template parameter (after performing any
   necessary type deduction if the template parameter is declared with a
   placeholder type). The type is non-const even if the entity is a template
   parameter object (which is a const object). (since C++20)

2) If the argument is any other expression of type `T`, and

   a) if the value category of expression is *xvalue*, then decltype yields
      `T&&`;

   b) if the value category of expression is *lvalue*, then decltype yields
      `T&`;

   c) if the value category of expression is *prvalue*, then decltype yields
      `T`. If expression is a function call which returns a prvalue of class
      type or is a comma expression whose right operand is such a function call,
      a temporary object is not introduced for that prvalue. (until C++17) If
      expression is a prvalue other than a (possibly parenthesized) immediate
      invocation(since C++20), a temporary object is not materialized from that
      prvalue: such prvalue has no result object. (since C++17) Because no
      temporary object is created, the type need not be complete or have an
      available destructor, and can be abstract. This rule doesn't apply to
      sub-expressions: in `decltype(f(g()))`, `g()` must have a complete type,
      but `f()` need not.

Note that if the name of an object is parenthesized, it is treated as an
ordinary lvalue expression, thus `decltype(x)` and `decltype((x))` are often
different types.

`decltype` is useful when declaring types that are difficult or impossible to
declare using standard notation, like lambda-related types or types that depend
on template parameters.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_decltype` | 200707L | (C++11) | `decltype`

### Keywords

`decltype`

### Example

```cpp
#include <cassert>
#include <iostream>
#include <type_traits>

struct A { double x; };
const A* a;

decltype(a->x) y;       // type of y is double (declared type)
decltype((a->x)) z = y; // type of z is const double& (lvalue expression)

template<typename T, typename U>
auto add(T t, U u) -> decltype(t + u) // return type depends on template parameters
                                      // return type can be deduced since C++14
{
    return t + u;
}

const int& getRef(const int* p) { return *p; }
static_assert(std::is_same_v<decltype(getRef), const int&(const int*)>);
auto getRefFwdBad(const int* p) { return getRef(p); }
static_assert(std::is_same_v<decltype(getRefFwdBad), int(const int*)>,
    "Just returning auto isn't perfect forwarding.");
decltype(auto) getRefFwdGood(const int* p) { return getRef(p); }
static_assert(std::is_same_v<decltype(getRefFwdGood), const int&(const int*)>,
    "Returning decltype(auto) perfectly forwards the return type.");

// Alternatively:
auto getRefFwdGood1(const int* p) -> decltype(getRef(p)) { return getRef(p); }
static_assert(std::is_same_v<decltype(getRefFwdGood1), const int&(const int*)>,
    "Returning decltype(return expression) also perfectly forwards the return type.");

int main()
{
    int i = 33;
    decltype(i) j = i * 2;
    static_assert(std::is_same_v<decltype(i), decltype(j)>);
    assert(i == 33 && 66 == j);

    auto f = [i](int av, int bv) -> int { return av * bv + i; };
    auto h = [i](int av, int bv) -> int { return av * bv + i; };
    static_assert(!std::is_same_v<decltype(f), decltype(h)>,
        "The type of a lambda function is unique and unnamed");

    decltype(f) g = f;
    std::cout << f(3, 3) << ' ' << g(3, 3) << '\n';
}
```

Output:

```text
42 42
```

### See also

- **`auto` specifier (C++11)** — specifies a type deduced from an expression
- **declval (C++11)** — obtains a reference to its argument for use in
  unevaluated context (function template)
- **is_same (C++11)** — checks if two types are the same (class template)

**C documentation for `typeof`**

---
*Source: https://en.cppreference.com/w/cpp/language/decltype*
