# Variable template (since C++14)

A variable template defines a family of variables or static data members.

### Syntax

```text
template < parameter-list > variable-declaration
```

- **variable-declaration** — a declaration of a variable. The declared variable
  name becomes a template name.
- **parameter-list** — a non-empty comma-separated list of the template
  parameters, each of which is either non-type parameter, a type parameter, a
  template parameter, or a parameter pack of any of those.

### Explanation

A variable instantiated from a variable template is called an *instantiated
variable*. A static data member instantiated from a static data member template
is called an *instantiated static data member*.

A variable template may be introduced by a template declaration at namespace
scope, where variable-declaration declares a variable.

```cpp
template<class T>
constexpr T pi = T(3.1415926535897932385L); // variable template

template<class T>
T circular_area(T r) // function template
{
    return pi<T> * r * r; // pi<T> is a variable template instantiation
}
```

When used at class scope, variable template declares a static data member
template.

```cpp
using namespace std::literals;
struct matrix_constants
{
    template<class T>
    using pauli = hermitian_matrix<T, 2>; // alias template

    template<class T> // static data member template
    static constexpr pauli<T> sigmaX = {{0, 1}, {1, 0}};

    template<class T>
    static constexpr pauli<T> sigmaY = {{0, -1i}, {1i, 0}};

    template<class T>
    static constexpr pauli<T> sigmaZ = {{1, 0}, {0, -1}};
};
```

As with other static members, a definition of a static data member template may
be required. Such definition is provided outside the class definition. A
template declaration of a static data member at namespace scope may also be a
definition of a non-template data member of a class template:

```cpp
struct limits
{
    template<typename T>
    static const T min; // declaration of a static data member template
};

template<typename T>
const T limits::min = { }; // definition of a static data member template

template<class T>
class X
{
    static T s; // declaration of a non-template static data member of a class template
};

template<class T>
T X<T>::s = 0; // definition of a non-template data member of a class template
```

Unless a variable template was explicitly specialized or explicitly
instantiated, it is implicitly instantiated when a specialization of the
variable template is referenced in a context that requires a variable definition
to exist or if the existence of the definition affects the semantics of the
program, i.e. if the variable is needed for constant evaluation by an expression
(the definition may be not used).

The existence of a definition of a variable is considered to affect the
semantics of the program if the variable is needed for constant evaluation by an
expression, even if constant evaluation of the expression is not required or if
constant expression evaluation does not use the definition.

### Notes

Until variable templates were introduced in C++14, parametrized variables were
typically implemented as either static data members of class templates or as
constexpr function templates returning the desired values.

Variable templates cannot be used as template template arguments.

  Feature-test macro | Value | Std | Feature
  `__cpp_variable_templates` | 201304L | (C++14) | Variable templates

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 2255 | C++14 | it was unclear whether a specialization of a static data
      member template is a static data member | it is

---
*Source: https://en.cppreference.com/w/cpp/language/variable_template*
