# Elaborated type specifier

Elaborated type specifiers may be used to refer to a previously-declared class
name (class, struct, or union) or to a previously-declared enum name even if the
name was hidden by a non-type declaration. They may also be used to declare new
class names.

### Syntax

```text
class-key class-name    (1)
enum enum-name    (2)
class-key attr ﻿(optional) identifier ;    (3)
```

- **class-key** — one of class, struct, union
- **class-name** — the name of a previously-declared class type, optionally
  qualified, or an identifier not previously declared as a type name
- **enum-name** — the name of a previously-declared enumeration type, optionally
  qualified
- **attr** — (since C++11) any number of attributes

1) Elaborated type specifier for a class type.

2) Elaborated type specifier for an enumeration type.

3) A declaration that consists solely of an elaborated type specifier always
   declares a class type named by identifier in the scope that contains the
   declaration.

Opaque enum declaration resembles form (3), but the enum type is a complete type
after an opaque enum declaration.

### Explanation

Form (3) is a special case of elaborated type specifier, usually referred to as
*forward declaration* of classes, for the description of form (3), see Forward
declaration. The following only apply to form (1) and (2).

The class-name or enum-name in the elaborated type specifier may either be a
simple identifier or be a qualified-id. The name is looked up using unqualified
name lookup or qualified name lookup, depending on their appearance. But in
either case, non-type names are not considered.

```cpp
class T
{
public:
    class U;
private:
    int U;
};

int main()
{
    int T;
    T t; // error: the local variable T is found
    class T t; // OK: finds ::T, the local variable T is ignored
    T::U* u; // error: lookup of T::U finds the private data member
    class T::U* u; // OK: the data member is ignored
}
```

If the name lookup does not find a previously declared type name, the
elaborated-type-specifier is introduced by `class`, `struct`, or `union` (i.e.
not by `enum`), and class-name is an unqualified identifier, then the
elaborated-type-specifier is a class declaration of the class-name.

```cpp
template<typename T>
struct Node
{
    struct Node* Next; // OK: lookup of Node finds the injected-class-name
    struct Data* Data; // OK: declares type Data at global scope
                       // and also declares the data member Data
    friend class ::List; // error: cannot introduce a qualified name
    enum Kind* kind; // error: cannot introduce an enum
};

Data* p; // OK: struct Data has been declared
```

If the name refers to a typedef name, a type alias, a template type parameter,
or an alias template specialization, the program is ill-formed, otherwise the
elaborated type specifier introduces the name into the declaration the same way
a simple type specifier introduces its type-name.

```cpp
template<typename T>
class Node
{
    friend class T; // error: type parameter cannot appear in an elaborated type specifier
};

class A {};
enum b { f, t };

int main()
{
    class A a; // OK: equivalent to 'A a;'
    enum b flag; // OK: equivalent to 'b flag;'
}
```

The class-key or `enum` keyword present in the elaborated-type-specifier must
agree in kind with the declaration to which the name in the
elaborated-type-specifier refers.

- the `enum` keyword must be used to refer to an enumeration type (whether
  scoped or unscoped)
- the `union` class-key must be used to refer to a union
- either the `class` or `struct` class-key must be used to refer to a non-union
  class type (the keywords `class` and `struct` are interchangeable here).

```cpp
enum class E { a, b };
enum E x = E::a; // OK
enum class E y = E::b; // error: 'enum class' cannot introduce an elaborated type specifier

struct A {};
class A a; // OK
```

When used as a template argument, `class T` is a type template parameter named
`T`, not an unnamed non-type parameter whose type `T` is introduced by
elaborated type specifier.

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++98 standard (ISO/IEC 14882:1998):

---
*Source: https://en.cppreference.com/w/cpp/language/elaborated_type_specifier*
