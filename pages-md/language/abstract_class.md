# Abstract class

Defines an abstract type which cannot be instantiated, but can be used as a base
class.

### Syntax

A *pure virtual* function is a virtual function whose declarator has the
following syntax:

```text
declarator virt-specifier ﻿(optional) = 0
```

Here the sequence `= 0` is known as pure-specifier, and appears either
immediately after the declarator or after the optional virt-specifier
(`override` or `final`).

pure-specifier cannot appear in a member function definition or friend
declaration.

```cpp
struct Base
{
    virtual int g();
    virtual ~Base() {}
};

struct A : Base
{
    // OK: declares three member virtual functions, two of them pure
    virtual int f() = 0, g() override = 0, h();

    // OK: destructor can be pure too
    ~A() = 0;

    // Error: pure-specifier on a function definition
    virtual int b() = 0 {}
};
```

An *abstract class* is a class that either defines or inherits at least one
function for which the final overrider is *pure virtual*.

### Explanation

Abstract classes are used to represent general concepts (for example, Shape,
Animal), which can be used as base classes for concrete classes (for example,
Circle, Dog).

No objects of an abstract class can be created (except for base subobjects of a
class derived from it) and no non-static data members whose type is an abstract
class can be declared.

Abstract types cannot be used as parameter types, as function return types, or
as the type of an explicit conversion (note this is checked at the point of
definition and function call, since at the point of function declaration
parameter and return type may be incomplete).

Pointers and references to an abstract class can be declared.

```cpp
struct Abstract
{
    virtual void f() = 0;  // pure virtual
}; // "Abstract" is abstract

struct Concrete : Abstract
{
    void f() override {}   // non-pure virtual
    virtual void g();      // non-pure virtual
}; // "Concrete" is non-abstract

struct Abstract2 : Concrete
{
    void g() override = 0; // pure virtual overrider
}; // "Abstract2" is abstract

int main()
{
    // Abstract a;   // Error: abstract class
    Concrete b;      // OK
    Abstract& a = b; // OK to reference abstract base
    a.f();           // virtual dispatch to Concrete::f()
    // Abstract2 a2; // Error: abstract class (final overrider of g() is pure)
}
```

The definition of a pure virtual function may be provided (and must be provided
if the pure virtual is the destructor): the member functions of the derived
class are free to call the abstract base's pure virtual function using qualified
function id. This definition must be provided outside of the class body (the
syntax of a function declaration doesn't allow both the pure specifier `= 0` and
a function body).

Making a virtual call to a pure virtual function from a constructor or the
destructor of the abstract class is undefined behavior (regardless of whether it
has a definition or not).

```cpp
struct Abstract
{
    virtual void f() = 0; // pure virtual
    virtual void g() {}   // non-pure virtual

    ~Abstract()
    {
        g();           // OK: calls Abstract::g()
        // f();        // undefined behavior
        Abstract::f(); // OK: non-virtual call
    }
};

// definition of the pure virtual function
void Abstract::f()
{
    std::cout << "A::f()\n";
}

struct Concrete : Abstract
{
    void f() override
    {
        Abstract::f(); // OK: calls pure virtual function
    }

    void g() override {}

    ~Concrete()
    {
        g(); // OK: calls Concrete::g()
        f(); // OK: calls Concrete::f()
    }
};
```

### Defect Reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 390 | C++98 | an undefined pure virtual destructor might be called | a
      definition is required in this case
  CWG 2153 | C++98 | pure-specifier could appear in friend declarations |
      prohibited

### See also

- `virtual`

---
*Source: https://en.cppreference.com/w/cpp/language/abstract_class*
