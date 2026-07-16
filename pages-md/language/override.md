# `override` specifier (since C++11)

Specifies that a virtual function overrides another virtual function.

### Syntax

The identifier `override`, if used, appears immediately after the declarator in
the syntax of a member function declaration or a member function definition
inside a class definition.

```text
declarator virt-specifier-seq ﻿(optional) pure-specifier ﻿(optional)    (1)
declarator virt-specifier-seq ﻿(optional) function-body    (2)
```

1) In a member function declaration, `override` may appear in virt-specifier-seq
   immediately after the declarator, and before the pure-specifier, if used.

2) In a member function definition inside a class definition, `override` may
   appear in virt-specifier-seq immediately after the declarator and just before
   function-body.

In both cases, virt-specifier-seq, if used, is either `override` or `final`, or
`final override` or `override final`.

### Explanation

In a member function declaration or definition, `override` specifier ensures
that the function is virtual and is overriding a virtual function from a base
class. The program is ill-formed (a compile-time error is generated) if this is
not true.

`override` is an identifier with a special meaning when used after member
function declarators: it's not a reserved keyword otherwise.

### Example

```cpp
#include <iostream>

struct A
{
    virtual void foo();
    void bar();
    virtual ~A();
};

// member functions definitions of struct A:
void A::foo() { std::cout << "A::foo();\n"; }
A::~A() { std::cout << "A::~A();\n"; }

struct B : A
{
//  void foo() const override; // Error: B::foo does not override A::foo
                               // (signature mismatch)
    void foo() override; // OK: B::foo overrides A::foo
//  void bar() override; // Error: A::bar is not virtual
    ~B() override; // OK: `override` can also be applied to virtual
                   // special member functions, e.g. destructors
    void override(); // OK, member function name, not a reserved keyword
};

// member functions definitions of struct B:
void B::foo() { std::cout << "B::foo();\n"; }
B::~B() { std::cout << "B::~B();\n"; }
void B::override() { std::cout << "B::override();\n"; }

int main()
{
    B b;
    b.foo();
    b.override(); // OK, invokes the member function `override()`
    int override{42}; // OK, defines an integer variable
    std::cout << "override: " << override << '\n';
}
```

Output:

```text
B::foo();
B::override();
override: 42
B::~B();
A::~A();
```

### See also

- **`final` specifier(C++11)** — declares that a method cannot be overridden

---
*Source: https://en.cppreference.com/w/cpp/language/override*
