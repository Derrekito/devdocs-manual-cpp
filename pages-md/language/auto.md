# Placeholder type specifiers (since C++11)

For variables, specifies that the type of the variable that is being declared
will be automatically deduced from its initializer.

For functions, specifies that the return type will be deduced from its return
statements.
*(since C++14)*

For non-type template parameters, specifies that the type will be deduced from
the argument.
*(since C++17)*

### Syntax

```text
type-constraint ﻿(optional) auto    (1)    (since C++11)
type-constraint ﻿(optional) decltype ( auto )    (2)    (since C++14)
```

- **type-constraint** — (since C++20) a concept name, optionally qualified,
  optionally followed by a template argument list enclosed in `<>`

1) type is deduced using the rules for template argument deduction.

2) type is `decltype(expr)`, where `expr` is the initializer or ones used in
   return statements.

The placeholder `auto` may be accompanied by modifiers, such as `const` or `&`,
which will participate in the type deduction. The placeholder `decltype(auto)`
must be the sole constituent of the declared type.(since C++14)

### Explanation

A placeholder type specifier may appear in the following contexts:

- in the type specifier sequence of a variable: `auto x = expr;` as a type
  specifier. The type is deduced from the initializer. If the placeholder type
  specifier is `auto` or type-constraint `auto`(since C++20), the variable type
  is deduced from the initializer using the rules for template argument
  deduction from a function call (see template argument deduction — other
  contexts for details). For example, given `const auto& i = expr;`, the type of
  `i` is exactly the type of the argument `u` in an imaginary template
  `template<class U> void f(const U& u)` if the function call `f(expr)` was
  compiled. Therefore, `auto&&` may be deduced either as an lvalue reference or
  rvalue reference according to the initializer, which is used in range-based
  for loop. If the placeholder type specifier is `decltype(auto)` or
  type-constraint `decltype(auto)`(since C++20), the deduced type is
  `decltype(expr)`, where `expr` is the initializer. (since C++14) If the
  placeholder type specifier is used to declare multiple variables, the deduced
  types must match. For example, the declaration `auto i = 0, d = 0.0;` is
  ill-formed, while the declaration `auto i = 0, *p = &i;` is well-formed and
  the `auto` is deduced as `int`.
- in the type-id of a new expression. The type is deduced from the initializer.
  For `new T init` (where `T` contains a placeholder type, *init* is either a
  parenthesized initializer or a brace-enclosed initializer list), the type of
  `T` is deduced as if for variable `x` in the invented declaration `T x init;`.
- (since C++14) in the return type of a function or lambda expression: `auto&
  f();`. The return type is deduced from the operand of its non-discarded(since
  C++17) return statement. See function — return type deduction.
- (since C++17) in the parameter declaration of a non-type template parameter:
  `template<auto I> struct A;`. Its type is deduced from the corresponding
  argument.

The `auto` specifier may also appear in the simple type specifier of an explicit
type conversion: `auto(expr)` and `auto{expr}`. Its type is deduced from the
expression.
*(since C++23)*

Furthermore, `auto` and type-constraint `auto`(since C++20) can appear in:
- the parameter declaration of a lambda expression: `[](auto&&){}`. Such lambda
  expression is a *generic lambda*.
- (since C++20) a function parameter declaration: `void f(auto);`. The function
  declaration introduces an abbreviated function template.
*(since C++14)*

#### Type constraint
If type-constraint is present, let `T` be the type deduced for the placeholder,
the type-constraint introduces a constraint expression as follows:
- If type-constraint is `Concept<A1, ..., An>`, then the constraint expression
  is `Concept<T, A1, ..., An>`;
- otherwise (type-constraint is `Concept` without an argument list), the
  constraint expression is `Concept<T>`.
Deduction fails if the constraint expression is invalid or returns `false`.
*(since C++20)*

### Notes

Until C++11, `auto` had the semantic of a storage duration specifier.

Mixing `auto` variables and functions in one declaration, as in `auto f() ->
int, i = 0;` is not allowed.

The `auto` specifier may also be used with a function declarator that is
followed by a trailing return type, in which case the declared return type is
that trailing return type (which may again be a placeholder type).

```cpp
auto (*p)() -> int; // declares p as pointer to function returning int
auto (*q)() -> auto = p; // declares q as pointer to function returning T
                         // where T is deduced from the type of p
```

The `auto` specifier may also be used in a structured binding declaration.
*(since C++17)*

The `auto` keyword may also be used in a nested-name-specifier. A
nested-name-specifier of the form `auto::` is a placeholder that is replaced by
a class or enumeration type following the rules for constrained type placeholder
deduction.
*(concepts TS)*

  Feature-test macro | Value | Std | Feature
  `__cpp_decltype_auto` | 201304L | (C++14) | `decltype(auto)`

### Example

```cpp
#include <iostream>
#include <utility>

template<class T, class U>
auto add(T t, U u) { return t + u; } // the return type is the type of operator+(T, U)

// perfect forwarding of a function call must use decltype(auto)
// in case the function it calls returns by reference
template<class F, class... Args>
decltype(auto) PerfectForward(F fun, Args&&... args)
{
    return fun(std::forward<Args>(args)...);
}

template<auto n> // C++17 auto parameter declaration
auto f() -> std::pair<decltype(n), decltype(n)> // auto can't deduce from brace-init-list
{
    return {n, n};
}

int main()
{
    auto a = 1 + 2;          // type of a is int
    auto b = add(1, 1.2);    // type of b is double
    static_assert(std::is_same_v<decltype(a), int>);
    static_assert(std::is_same_v<decltype(b), double>);

    auto c0 = a;             // type of c0 is int, holding a copy of a
    decltype(auto) c1 = a;   // type of c1 is int, holding a copy of a
    decltype(auto) c2 = (a); // type of c2 is int&, an alias of a
    std::cout << "before modification through c2, a = " << a << '\n';
    ++c2;
    std::cout << " after modification through c2, a = " << a << '\n';

    auto [v, w] = f<0>(); //structured binding declaration

    auto d = {1, 2}; // OK: type of d is std::initializer_list<int>
    auto n = {5};    // OK: type of n is std::initializer_list<int>
//  auto e{1, 2};    // Error as of DR n3922, std::initializer_list<int> before
    auto m{5};       // OK: type of m is int as of DR n3922, initializer_list<int> before
//  decltype(auto) z = { 1, 2 } // Error: {1, 2} is not an expression

    // auto is commonly used for unnamed types such as the types of lambda expressions
    auto lambda = [](int x) { return x + 3; };

//  auto int x; // valid C++98, error as of C++11
//  auto x;     // valid C, error in C++

    [](...){}(c0, c1, v, w, d, n, m, lambda); // suppresses "unused variable" warnings
}
```

Possible output:

```text
before modification through c2, a = 3
 after modification through c2, a = 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1265 | C++11 | the `auto` specifier could be used to declare a function
      with a trailing return type and define a variable in one declaration
      statement | prohibited
  CWG 1346 | C++11 | a parenthesized expression list could not be assigned to an
      auto variable | allowed
  CWG 1347 | C++11 | a declaration with the `auto` specifier could define two
      variables with types `T` and `std::initializer_list<T>` respectively |
      prohibited
  CWG 1852 | C++14 | the `auto` specifier in `decltype(auto)` was also a
      placeholder | not a placeholder in this case

---
*Source: https://en.cppreference.com/w/cpp/language/auto*
