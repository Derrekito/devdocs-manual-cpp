# Type alias, alias template (since C++11)

Type alias is a name that refers to a previously defined type (similar to
`typedef`).

Alias template is a name that refers to a family of types.

### Syntax

Alias declarations are declarations with the following syntax:

```text
using identifier attr ﻿(optional) = type-id ;    (1)
template < template-parameter-list > using identifier attr ﻿(optional) = type-id ;    (2)
```

- **attr** — optional sequence of any number of attributes
- **identifier** — the name that is introduced by this declaration, which
  becomes either a type name (1) or a template name (2)
- **template-parameter-list** — template parameter list, as in template
  declaration
- **type-id** — abstract declarator or any other valid type-id (which may
  introduce a new type, as noted in type-id). The type-id cannot directly or
  indirectly refer to identifier. Note that the point of declaration of the
  identifier is at the semicolon following type-id.

### Explanation

1) A type alias declaration introduces a name which can be used as a synonym for
   the type denoted by type-id. It does not introduce a new type and it cannot
   change the meaning of an existing type name. There is no difference between a
   type alias declaration and typedef declaration. This declaration may appear
   in block scope, class scope, or namespace scope.

2) An alias template is a template which, when specialized, is equivalent to the
   result of substituting the template arguments of the alias template for the
   template parameters in the type-id. template<class T> struct Alloc {};
   template<class T> using Vec = vector<T, Alloc<T>>; // type-id is vector<T,
   Alloc<T>> Vec<int> v; // Vec<int> is the same as vector<int, Alloc<int>> When
   the result of specializing an alias template is a dependent template-id,
   subsequent substitutions apply to that template-id: template<typename...>
   using void_t = void; template<typename T> void_t<typename T::foo> f();
   f<int>(); // error, int does not have a nested type foo The type produced
   when specializing an alias template is not allowed to directly or indirectly
   make use of its own type: template<class T> struct A; template<class T> using
   B = typename A<T>::U; // type-id is A<T>::U template<class T> struct A {
   typedef B<T> U; }; B<short> b; // error: B<short> uses its own type via
   A<short>::U Alias templates are never deduced by template argument deduction
   when deducing a template template parameter. It is not possible to partially
   or explicitly specialize an alias template.

Like any template declaration, an alias template can only be declared at class
scope or namespace scope.

The type of a lambda expression appearing in an alias template declaration is
different between instantiations of that template, even when the lambda
expression is not dependent.
```cpp
template<class T>
using A = decltype([] {}); // A<int> and A<char> refer to different closure types
```
*(since C++20)*

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_alias_templates` | 200704L | (C++11) | Alias templates

### Example

```cpp
#include <iostream>
#include <string>
#include <type_traits>
#include <typeinfo>

// type alias, identical to
// typedef std::ios_base::fmtflags flags;
using flags = std::ios_base::fmtflags;
// the name 'flags' now denotes a type:
flags fl = std::ios_base::dec;

// type alias, identical to
// typedef void (*func)(int, int);
using func = void (*) (int, int);

// the name 'func' now denotes a pointer to function:
void example(int, int) {}
func f = example;

// alias template
template<class T>
using ptr = T*;
// the name 'ptr<T>' is now an alias for pointer to T
ptr<int> x;

// type alias used to hide a template parameter
template<class CharT>
using mystring = std::basic_string<CharT, std::char_traits<CharT>>;

mystring<char> str;

// type alias can introduce a member typedef name
template<typename T>
struct Container { using value_type = T; };

// which can be used in generic programming
template<typename ContainerT>
void info(const ContainerT& c)
{
    typename ContainerT::value_type T;
    std::cout << "ContainerT is `" << typeid(decltype(c)).name() << "`\n"
                 "value_type is `" << typeid(T).name() << "`\n";
}

// type alias used to simplify the syntax of std::enable_if
template<typename T>
using Invoke = typename T::type;

template<typename Condition>
using EnableIf = Invoke<std::enable_if<Condition::value>>;

template<typename T, typename = EnableIf<std::is_polymorphic<T>>>
int fpoly_only(T) { return 1; }

struct S { virtual ~S() {} };

int main()
{
    Container<int> c;
    info(c); // Container::value_type will be int in this function
//  fpoly_only(c); // error: enable_if prohibits this
    S s;
    fpoly_only(s); // okay: enable_if allows this
}
```

Possible output:

```text
ContainerT is `struct Container<int>`
value_type is `int`
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1558 | C++11 | whether unused arguments in an alias specialization
      participate in substitution is not specified | substitution is performed

### See also

- **`typedef` declaration** — creates a synonym for a type
- **namespace alias** — creates an alias of an existing namespace

---
*Source: https://en.cppreference.com/w/cpp/language/type_alias*
