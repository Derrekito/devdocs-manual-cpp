# Conflicting declarations

Unless otherwise specified, two declarations cannot (re)introduce the same
entity. The program is ill-formed if such declarations exist.

### Corresponding declarations

Two declarations *correspond* if they (re)introduce the same name, both declare
constructors, or both declare destructors, unless

- either is a using declaration,
- one declares a type (not a typedef name) and the other declares a variable,
  non-static data member other than of an anonymous union, enumerator, function,
  or function template, or
- each declares a function or function template and they do not declare
  corresponding overloads (see below).

Two function declarations declare *corresponding overloads* if both declare
functions satisfying all following conditions:

- They have the same parameter-type-list, omitting the types of explicit object
  parameters(since C++23).

- They have equivalent trailing requires clauses (if any, except friend
  declarations).
*(since C++20)*

- If both of them are non-static member functions, they need to additionally
  satisfy one of the following requirements:

- Exactly one of them is an implicit object member function without
  ref-qualifier and the types of their object parameters, after removing
  top-level references, are the same.
*(since C++23)*

- Their object parameters have the same type.

Two function template declarations declare *corresponding overloads* if both
declare function templates satisfying all following conditions:

- Their template parameter lists have the same length.
- Their corresponding template parameters are equivalent.
- They have equivalent parameter-type-lists, omitting the types of explicit
  object parameters(since C++23).
- They have equivalent return types.

- Their corresponding template parameters are either both declared without
  constraint, or both declared with equivalent constraints.
- They have equivalent trailing requires clauses (if any).
*(since C++20)*

- If both are non-static members function templates, they need to additionally
  satisfy one of the following requirements:

- Exactly one of them is an implicit object member function template without
  ref-qualifier and the types of their object parameters, after removing all
  references, are equivalent.
*(since C++23)*

- Their object parameters have equivalent types.

```cpp
struct A
{
    friend void c();   // #1
};

struct B
{
    friend void c() {} // corresponds to, and defines, #1
};

typedef int Int;

enum E : int { a };

void f(int);   // #2
void f(Int) {} // defines #2
void f(E) {}   // OK, another overload

struct X
{
    static void f();
    void f() const;   // error: redeclaration

    void g();
    void g() const;   // OK
    void g() &;       // error: redeclaration

    void h(this X&, int);
    void h(int) &&;   // OK, another overload

    void j(this const X&);
    void j() const &; // error: redeclaration

    void k();
    void k(this X&);  // error: redeclaration
};
```

### Potentially-conflict declarations

A declaration is *name-independent* if its name is `_` and it declares
- a variable with automatic storage duration,
- a structured binding not inhabiting a namespace scope,
- the variable introduced by a lambda capture with an initializer, or
- a non-static data member.
*(since C++26)*

Unless otherwise specified, two declarations of entities *declare the same
entity* if all following conditions are satisfied, considering declarations of
unnamed types to introduce their typedef names and enumeration names for linkage
purposes (if any exists):

- They correspond.
- They have the same target scope, which is not a function parameter scope or a
  template parameter scope.

- Neither is a name-independent declaration.
*(since C++26)*

- One of the following conditions is satisfied:

- They both declare names with module linkage and are attached to the same
  module.
*(since C++20)*

- They both declare names with external linkage.

Two declarations *potentially conflict* if they correspond and cause their
shared name to denote different entities. The program is ill-formed if, in any
scope, a name is bound to two declarations `A` and `B` that potentially conflict
and `A` precedes `B`, unless `B` is name-independent.(since C++26)

```cpp
void f()
{
    int x, y;
    void x(); // error: different entity for x
    int y;    // error: redefinition
}

enum { f };   // error: different entity for ::f

namespace A {}
namespace B = A;
namespace B = A; // OK, no effect
namespace B = B; // OK, no effect
namespace A = B; // OK, no effect
namespace B {} // error: different entity for B

void g()
{
    int _;
    _ = 0; // OK
    int _; // OK since C++26, name-independent declaration
    _ = 0; // error: two non-function declarations in the lookup set
}

void h ()
{
    int _;        // #1
    _ ++;         // OK
    static int _; // error: conflicts with #1 because
                  // static variables are not name-independent
}
```

---
*Source: https://en.cppreference.com/w/cpp/language/conflicting_declarations*
