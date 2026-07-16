# Member templates

Template declarations (class, function, and variables(since C++14)) can appear
inside a member specification of any class, struct, or union that are not local
classes.

```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>

struct Printer
{
    // generic functor
    std::ostream& os;
    Printer(std::ostream& os) : os(os) {}
    template<typename T>
    void operator()(const T& obj) { os << obj << ' '; } // member template
};

int main()
{
    std::vector<int> v{1,2,3};
    std::for_each(v.begin(), v.end(), Printer(std::cout));
    std::string s{"abc"};
    std::ranges::for_each(s, Printer(std::cout));
}
```

Output:

```text
1 2 3 a b c
```

Partial specializations of member template may appear both at class scope and at
enclosing namespace scope. Explicit specializations may appear in any scope in
which the primary template may appear.

```cpp
struct A
{
    template<class T> struct B;        // primary member template
    template<class T> struct B<T*> {}; // OK: partial specialization
//  template<> struct B<int*> {};      // OK via CWG 727: full specialization
};
template<> struct A::B<int*> {};       // OK
template<class T> struct A::B<T&> {};  // OK
```

If the enclosing class declaration is, in turn, a class template, when a member
template is defined outside of the class body, it takes two sets of template
parameters: one for the enclosing class, and another one for itself:

```cpp
template<typename T1>
struct string
{
    // member template function
    template<typename T2>
    int compare(const T2&);
    // constructors can be templates too
    template<typename T2>
    string(const std::basic_string<T2>& s) { /*...*/ }
};
// out of class definition of string<T1>::compare<T2>
template<typename T1> // for the enclosing class template
template<typename T2> // for the member template
int string<T1>::compare(const T2& s) { /* ... */ }
```

### Member function templates

Destructors and copy constructors cannot be templates. If a template constructor
is declared which could be instantiated with the type signature of a copy
constructor, the implicitly-declared copy constructor is used instead.

A member function template cannot be virtual, and a member function template in
a derived class cannot override a virtual member function from the base class.

```cpp
class Base
{
    virtual void f(int);
};

struct Derived : Base
{
    // this member template does not override Base::f
    template<class T> void f(T);

    // non-template member override can call the template:
    void f(int i) override
    {
         f<>(i);
    }
};
```

A non-template member function and a template member function with the same name
may be declared. In case of conflict (when some template specialization matches
the non-template function signature exactly), the use of that name and type
refers to the non-template member unless an explicit template argument list is
supplied.

```cpp
template<typename T>
struct A
{
    void f(int); // non-template member

    template<typename T2>
    void f(T2); // member template
};

// template member definition
template<typename T>
template<typename T2>
void A<T>::f(T2)
{
    // some code
}

int main()
{
    A<char> ac;
    ac.f('c'); // calls template function A<char>::f<char>(char)
    ac.f(1);   // calls non-template function A<char>::f(int)
    ac.f<>(1); // calls template function A<char>::f<int>(int)
}
```

An out-of-class definition of a member function template must be *equivalent* to
the declaration inside the class (see function template overloading for the
definition of equivalency), otherwise it is considered to be an overload.

```cpp
struct X
{
    template<class T> T good(T n);
    template<class T> T bad(T n);
};

template<class T> struct identity { using type = T; };

// OK: equivalent declaration
template<class V>
V X::good(V n) { return n; }

// Error: not equivalent to any of the declarations inside X
template<class T>
T X::bad(typename identity<T>::type n) { return n; }
```

### Conversion function templates

A user-defined conversion function can be a template.

```cpp
struct A
{
    template<typename T>
    operator T*(); // conversion to pointer to any type
};

// out-of-class definition
template<typename T>
A::operator T*() { return nullptr; }

// explicit specialization for char*
template<>
A::operator char*() { return nullptr; }

// explicit instantiation
template A::operator void*();

int main()
{
    A a;
    int* ip = a.operator int*(); // explicit call to A::operator int*()
}
```

During overload resolution, specializations of conversion function templates are
not found by name lookup. Instead, all visible conversion function templates are
considered, and every specialization produced by template argument deduction
(which has special rules for conversion function templates) is used as if found
by name lookup.

Using-declarations in derived classes cannot refer to specializations of
template conversion functions from base classes.

A user-defined conversion function template cannot have a deduced return type:
```cpp
struct S
{
    operator auto() const { return 10; } // OK
    template<class T> operator auto() const { return 42; } // error
};
```
*(since C++14)*

### Member variable templates
A variable template declaration may appear at class scope, in which case it
declares a static data member template. See variable templates for details.
*(since C++14)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1878 | C++14 | operator auto was technically allowed | operator auto
      forbidden

---
*Source: https://en.cppreference.com/w/cpp/language/member_template*
