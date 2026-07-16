# Non-static member functions

A non-static member function is a function that is declared in a member
specification of a class without a `static` or `friend` specifier (see static
member functions and friend declaration for the effect of those keywords).

```cpp
class S
{
    int mf1(); // non-static member function declaration
    void mf2() volatile, mf3() &&; // can have cv-qualifiers and/or a reference-qualifier
        // the declaration above is equivalent to two separate declarations:
        // void mf2() volatile;
        // void mf3() &&;

    int mf4() const { return data; } // can be defined inline
    virtual void mf5() final; // can be virtual, can use final/override
    S() : data(12) {} // constructors are member functions too
    int data;
};

int S::mf1() { return 7; } // if not defined inline, has to be defined at namespace
```

Constructors, destructors, and conversion functions use special syntaxes for
their declarations. The rules described in this page may not apply to these
functions. See their respective pages for details.

An *explicit object member function* is a non-static member function with an
explicit object parameter.
*(since C++23)*

An *implicit object member function* is a non-static member function without an
explicit object parameter (prior to C++23, this was the only kind of non-static
member function, and hence referred to as "non-static member function" in the
literature).

### Explanation

Any function declarations are allowed, with additional syntax elements that are
only available for non-static member functions: pure-specifiers, cv-qualifiers,
ref-qualifiers, `final` and `override` specifiers(since C++11), and member
initialization lists.

A non-static member function of class `X` may be called

1) For an object of type `X` using the class member access operator

2) For an object of a class derived from `X`

3) Directly from within the body of a member function of `X`

4) Directly from within the body of a member function of a class derived from
   `X`

Calling a non-static member function of class `X` on an object that is not of
type `X`, or of a type derived from `X` invokes undefined behavior.

Within the body of a non-static member function of `X`, any id-expression `e`
(e.g. an identifier) that resolves to a non-type non-static member of `X` or of
a base class of `X`, is transformed to a member access expression `(*this).e`
(unless it's already a part of a member access expression). This does not occur
in template definition context, so a name may have to be prefixed with `this->`
explicitly to become dependent.

```cpp
struct S
{
    int n;
    void f();
};

void S::f()
{
    n = 1; // transformed to (*this).n = 1;
}

int main()
{
    S s1, s2;
    s1.f(); // changes s1.n
}
```

Within the body of a non-static member function of `X`, any unqualified-id that
resolves to a static member, an enumerator or a nested type of `X` or of a base
class of `X`, is transformed to the corresponding qualified-id:

```cpp
struct S
{
    static int n;
    void f();
};

void S::f()
{
    n = 1; // transformed to S::n = 1;
}

int main()
{
    S s1, s2;
    s1.f(); // changes S::n
}
```

#### Member functions with cv-qualifiers

An implicit object member function can be declared with a cv-qualifier sequence
(const, volatile, or a combination of const and volatile), this sequence appears
after the parameter list in the function declaration. Functions with different
cv-qualifier sequences (or no sequence) have different types and so may overload
each other.

In the body of a function with a cv-qualifier sequence, `*this` is cv-qualified,
e.g. in a member function with const qualifier, only other member functions with
const qualifier may be called normally. A member function without const
qualifier may still be called if `const_cast` is applied or through an access
path that does not involve `this`.

```cpp
#include <vector>

struct Array
{
    std::vector<int> data;
    Array(int sz) : data(sz) {}

    // const member function
    int operator[](int idx) const
    {                     // the this pointer has type const Array*
        return data[idx]; // transformed to (*this).data[idx];
    }

    // non-const member function
    int& operator[](int idx)
    {                     // the this pointer has type Array*
        return data[idx]; // transformed to (*this).data[idx]
    }
};

int main()
{
    Array a(10);
    a[1] = 1;  // OK: the type of a[1] is int&
    const Array ca(10);
    ca[1] = 2; // Error: the type of ca[1] is int
}
```

#### Member functions with ref-qualifier
An implicit object member function can be declared with no ref-qualifier, with
an lvalue ref-qualifier (the token `&` after the parameter list) or the rvalue
ref-qualifier (the token `&&` after the parameter list). During overload
resolution, an implicit object member function with a cv-qualifier sequence of
class X is treated as follows:
- no ref-qualifier: the implicit object parameter has type lvalue reference to
  cv-qualified X and is additionally allowed to bind rvalue implied object
  argument
- lvalue ref-qualifier: the implicit object parameter has type lvalue reference
  to cv-qualified X
- rvalue ref-qualifier: the implicit object parameter has type rvalue reference
  to cv-qualified X
```cpp
#include <iostream>
struct S
{
    void f() &  { std::cout << "lvalue\n"; }
    void f() && { std::cout << "rvalue\n"; }
};
int main()
{
    S s;
    s.f();            // prints "lvalue"
    std::move(s).f(); // prints "rvalue"
    S().f();          // prints "rvalue"
}
```
Note: unlike cv-qualification, ref-qualification does not change the properties
of the `this` pointer: within an rvalue ref-qualified function, `*this` remains
an lvalue expression.
*(since C++11)*

#### Virtual and pure virtual functions

A non-static member function may be declared *virtual* or *pure virtual*. See
virtual functions and abstract classes for details.

#### Explicit object parameter
A non-static member function can be declared to take as its first parameter an
explicit object parameter, denoted with the prefixed keyword `this`.
```cpp
struct X
{
    void foo(this X const& self, int i); // same as void foo(int i) const &;
//  void foo(int i) const &; // Error: already declared
    void bar(this X self, int i); // pass object by value: makes a copy of `*this`
};
```
For member function templates, explicit object parameter allows deduction of
type and value category, this language feature is called "deducing `this`".
```cpp
struct X
{
    template<typename Self>
    void foo(this Self&&, int);
};
struct D : X {};
void ex(X& x, D& d)
{
    x.foo(1);       // Self = X&
    move(x).foo(2); // Self = X
    d.foo(3);       // Self = D&
}
```
This makes it possible to deduplicate const- and non-const member functions, see
array subscript operator for an example.
Additionally, explicit object parameter deduces to the derived type, which
simplifies CRTP:
```cpp
// a CRTP trait
struct add_postfix_increment
{
    template<typename Self>
    auto operator++(this Self&& self, int)
    {
        auto tmp = self; // Self deduces to "some_type"
        ++self;
        return tmp;
    }
};
struct some_type : add_postfix_increment
{
    some_type& operator++() { ... }
};
```
Inside the body of a function with explicit object parameter, the `this` pointer
cannot be used: all member access must be done through the first parameter, like
in static member functions:
```cpp
struct C
{
    void bar();
    void foo(this C c)
    {
        auto x = this; // error: no this
        bar();         // error: no implicit this->
        c.bar();       // ok
    }
};
```
A pointer to a member function with explicit object parameter is an ordinary
pointer to function, not a pointer to member:
```cpp
struct Y
{
    int f(int, int) const&;
    int g(this Y const&, int, int);
};
auto pf = &Y::f;
pf(y, 1, 2);              // error: pointers to member functions are not callable
(y.*pf)(1, 2);            // ok
std::invoke(pf, y, 1, 2); // ok
auto pg = &Y::g;
pg(y, 3, 4);              // ok
(y.*pg)(3, 4);            // error: pg is not a pointer to member function
std::invoke(pg, y, 3, 4); // ok
```
Member functions with an explicit object parameter cannot be static or virtual
and they cannot have cv- or ref-qualifiers.
*(since C++23)*

#### Special member functions

Some member functions are *special*: under certain circumstances they are
defined by the compiler even if not defined by the user. They are:

- Default constructor
- Copy constructor

- Move constructor
*(since C++11)*

- Copy assignment operator

- Move assignment operator
*(since C++11)*

- Destructor(until C++20)Prospective destructor(since C++20)

Special member functions along with the comparison operators(since C++20) are
the only functions that can be *defaulted*, that is, defined using = default
instead of the function body (see their pages for details).

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_ref_qualifiers` | 200710L | (C++11) | ref-qualifiers
  `__cpp_explicit_this_parameter` | 202110L | (C++23) | Explicit object
      parameter

### Example

```cpp
#include <exception>
#include <iostream>
#include <string>
#include <utility>

struct S
{
    int data;

    // simple converting constructor (declaration)
    S(int val);

    // simple explicit constructor (declaration)
    explicit S(std::string str);

    // const member function (definition)
    virtual int getData() const { return data; }
};

// definition of the constructor
S::S(int val) : data(val)
{
    std::cout << "ctor1 called, data = " << data << '\n';
}

// this constructor has a catch clause
S::S(std::string str) try : data(std::stoi(str))
{
    std::cout << "ctor2 called, data = " << data << '\n';
}
catch(const std::exception&)
{
    std::cout << "ctor2 failed, string was '" << str << "'\n";
    throw; // ctor's catch clause should always rethrow
}

struct D : S
{
    int data2;
    // constructor with a default argument
    D(int v1, int v2 = 11) : S(v1), data2(v2) {}

    // virtual member function
    int getData() const override { return data * data2; }

    // lvalue-only assignment operator
    D& operator=(D other) &
    {
        std::swap(other.data, data);
        std::swap(other.data2, data2);
        return *this;
    }
};

int main()
{
    D d1 = 1;
    S s2("2");

    try
    {
        S s3("not a number");
    }
    catch(const std::exception&) {}

    std::cout << s2.getData() << '\n';

    D d2(3, 4);
    d2 = d1;   // OK: assignment to lvalue
//  D(5) = d1; // ERROR: no suitable overload of operator=
}
```

Output:

```text
ctor1 called, data = 1
ctor2 called, data = 2
ctor2 failed, string was 'not a number'
2
ctor1 called, data = 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 194 | C++98 | ambiguous whether a non-static member function could have
      the same name as the enclosing class name | explicit naming restriction
      added

### See also

- classes
- non-static data members
- static data members

---
*Source: https://en.cppreference.com/w/cpp/language/member_functions*
