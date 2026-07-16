# Dependent names

Inside the definition of a template (both class template and function template),
the meaning of some constructs may differ from one instantiation to another. In
particular, types and expressions may depend on types of type template
parameters and values of non-type template parameters.

```cpp
template<typename T>
struct X : B<T> // "B<T>" is dependent on T
{
    typename T::A* pa; // "T::A" is dependent on T
                       // (see below for the meaning of this use of "typename")

    void f(B<T>* pb)
    {
        static int i = B<T>::i; // "B<T>::i" is dependent on T
        pb->j++; // "pb->j" is dependent on T
    }
};
```

Name lookup and binding are different for *dependent names* and non-*dependent
names*.

### Binding rules

Non-dependent names are looked up and bound at the point of template definition.
This binding holds even if at the point of template instantiation there is a
better match:

```cpp
#include <iostream>

void g(double) { std::cout << "g(double)\n"; }

template<class T>
struct S
{
    void f() const
    {
        g(1); // "g" is a non-dependent name, bound now
    }
};

void g(int) { std::cout << "g(int)\n"; }

int main()
{
    g(1);  // calls g(int)

    S<int> s;
    s.f(); // calls g(double)
}
```

If the meaning of a *non-dependent name* changes between the definition context
and the point of instantiation of a specialization of the template, the program
is ill-formed, no diagnostic required. This is possible in the following
situations:

- a type used in a non-dependent name is incomplete at the point of definition
  but complete at the point of instantiation

- lookup for a name in the template definition found a using-declaration, but
  the lookup in the corresponding scope in the instantiation does not find any
  declarations because the using-declaration was a pack expansion and the
  corresponding pack is empty
*(since C++17)*

- an instantiation uses a default argument or default template argument that had
  not been defined at the point of definition
- a constant expression at the point of instantiation uses the value of a const
  object of integral or unscoped enum type, the value of a constexpr object, the
  value of a reference, or the definition of a constexpr function(since C++11),
  and that object/reference/function(since C++11) was not defined at the point
  of definition
- the template uses a non-dependent class template specialization or variable
  template specialization(since C++14) at the point of instantiation, and this
  template it uses is either instantiated from a partial specialization that was
  not defined at the point of definition or names an explicit specialization
  that was not declared at the point of definition

Binding of *dependent names* is postponed until lookup takes place.

### Lookup rules

As discussed in lookup, the lookup of a dependent name used in a template is
postponed until the template arguments are known, at which time

- non-ADL lookup examines function declarations with external linkage that are
  visible from the *template definition* context
- ADL examines function declarations with external linkage that are visible from
  either the *template definition* context or the *template instantiation*
  context

(in other words, adding a new function declaration after template definition
does not make it visible, except via ADL).

The purpose of this rule is to help guard against violations of the ODR for
template instantiations:

```cpp
// an external library
namespace E
{
    template<typename T>
    void writeObject(const T& t)
    {
        std::cout << "Value = " << t << '\n';
    }
}

// translation unit 1:
// Programmer 1 wants to allow E::writeObject to work with vector<int>
namespace P1
{
    std::ostream& operator<<(std::ostream& os, const std::vector<int>& v)
    {
        for (int n : v)
            os << n << ' ';
        return os;
    }

    void doSomething()
    {
        std::vector<int> v;
        E::writeObject(v); // error: will not find P1::operator<<
    }
}

// translation unit 2:
// Programmer 2 wants to allow E::writeObject to work with vector<int>
namespace P2
{
    std::ostream& operator<<(std::ostream& os, const std::vector<int>& v)
    {
        for (int n : v)
            os << n << ':';
        return os << "[]";
    }

    void doSomethingElse()
    {
        std::vector<int> v;
        E::writeObject(v); // error: will not find P2::operator<<
    }
}
```

In the above example, if non-ADL lookup for `operator<<` were allowed from the
instantiation context, the instantiation of `E::writeObject<vector<int>>` would
have two different definitions: one using `P1::operator<<` and one using
`P2::operator<<`. Such ODR violation may not be detected by the linker, leading
to one or the other being used in both instances.

To make ADL examine a user-defined namespace, either `std::vector` should be
replaced by a user-defined class or its element type should be a user-defined
class:

```cpp
namespace P1
{
    // if C is a class defined in the P1 namespace
    std::ostream& operator<<(std::ostream& os, const std::vector<C>& v)
    {
        for (C n : v)
            os << n;
        return os;
    }

    void doSomething()
    {
        std::vector<C> v;
        E::writeObject(v); // OK: instantiates writeObject(std::vector<P1::C>)
                           //     which finds P1::operator<< via ADL
    }
}
```

Note: this rule makes it impractical to overload operators for standard library
types:

```cpp
#include <iostream>
#include <iterator>
#include <utility>
#include <vector>

// Bad idea: operator in global namespace, but its arguments are in std::
std::ostream& operator<<(std::ostream& os, std::pair<int, double> p)
{
    return os << p.first << ',' << p.second;
}

int main()
{
    typedef std::pair<int, double> elem_t;
    std::vector<elem_t> v(10);
    std::cout << v[0] << '\n'; // OK, ordinary lookup finds ::operator<<
    std::copy(v.begin(), v.end(),
              std::ostream_iterator<elem_t>(std::cout, " "));
    // Error: both ordinary lookup from the point of definition of
    // std::ostream_iterator and ADL will only consider the std namespace,
    // and will find many overloads of std::operator<<, so the lookup will be done.
    // Overload resolution will then fail to find operator<< for elem_t
    // in the set found by the lookup.
}
```

Note: limited lookup (but not binding) of dependent names also takes place at
template definition time, as needed to distinguish them from non-dependent names
and also to determine whether they are members of the current instantiation or
members of unknown specialization. The information obtained by this lookup can
be used to detect errors, see below.

### Dependent types

The following types are dependent types:

- template parameter
- a member of an *unknown specialization* (see below)
- a nested class/enum that is a dependent member of *unknown specialization*
  (see below)
- a cv-qualified version of a dependent type
- a compound type constructed from a dependent type
- an array type whose element type is dependent or whose bound (if any) is
  value-dependent

- a function type whose parameters include one or more function parameter packs
*(since C++11)*

- a function type whose exception specification is value-dependent
- a template-id where either

- the result of `decltype` applied to a type-dependent expression
The result of `decltype` applied to a type-dependent expression is a unique
dependent type. Two such results refer to the same type only if their
expressions are equivalent.
*(since C++11)*

- the pack indexing specifier applied to a type-dependent constant expression
The pack indexing specifier applied to a type-dependent constant expression is a
unique dependent type. Two such pack indexing specifiers refer to the same type
only if their constant expressions are equivalent. Otherwise, two such pack
indexing specifiers refer to the same type only if their indices have the same
value.
*(since C++26)*

Note: a typedef member of a current instantiation is only dependent when the
type it refers to is.

### Type-dependent expressions

The following expressions are type-dependent:

- an expression whose any subexpression is a type-dependent expression
- `this`, if the class is a dependent type.
- an id-expression that is not a concept-id and(since C++20)

- contains the special identifier `__func__` (if some enclosing function is a
  template, a non-template member of a class template, or a generic lambda(since
  C++14))
*(since C++11)*

- contains the name of conversion function to a dependent type
- contains a nested name specifier or qualified-id that is a member of *unknown
  specialization*
- names a dependent member of the *current instantiation* which is a static data
  member of type "array of unknown bound"

- contains an identifier for which name lookup finds one or more declarations of
  member functions of the current instantiation declared with return type
  deduction
*(since C++14)*

- contains an identifier for which name lookup finds a structured binding
  declaration whose initializer is type-dependent
- contains an identifier for which name lookup finds a non-type template
  parameter whose type contains the placeholder `auto`
- contains an identifier for which by name lookup finds a variable declared with
  a type that contains a placeholder type (e.g., `auto` static data member),
  where the initializer is type-dependent,
*(since C++17)*

- any cast expression to a dependent type
- new-expression that creates an object of a dependent type
- member access expression that refers to a member of the *current
  instantiation* whose type is dependent
- member access expression that refers to a member of *unknown specialization*

- fold-expression
*(since C++17)*

- pack indexing expression if its id-expression is a type-dependent expression
*(since C++26)*

Note: literals, pseudo-destructor calls, `alignof`, `noexcept`(since C++11),
`sizeof`, `typeid`, `delete`, and `throw`-expressions are never type-dependent
because the types of these expressions cannot be.

### Value-dependent expressions

- an expression used in context where constant expression is required, and whose
  any subexpression is value-dependent
- an id-expression that

- is a concept-id and any of its arguments are dependent
*(since C++20)*

- is type-dependent
- is a name of a non-type template parameter
- names a static data member that is a dependent member of the *current
  instantiation* and is not initialized.
- names a static member function that is a dependent member of the *current
  instantiation*
- is a constant with a integer or enumeration(until C++11)literal(since C++11)
  type, initialized from a value-dependent expression
- `alignof`, `noexcept`,(since C++11)`sizeof`, `typeid`-expressions where the
  argument is a type-dependent expression or a dependent type-id
- any cast expression to a dependent type or from a value-dependent expression
- address-of expression where the argument is qualified-id that names a
  dependent member of the *current instantiation*
- address-of expression where the argument is any expression which, evaluated as
  a core constant expression, refers to a templated entity that is an object
  with static or thread storage(since C++11) duration or a member function.

- fold-expression
*(since C++17)*

### Dependent names

### Current instantiation

Within a class template definition (including its member functions and nested
classes) some names may be deduced to refer to the *current instantiation*. This
allows certain errors to be detected at the point of definition, rather than
instantiation, and removes the requirement on the `typename` and `template`
disambiguators for dependent names, see below.

Only the following names can refer to the current instantiation:

- in a class template definition:
- in a primary class template definition or in the definition of its member:
- in the definition of a nested class or class template:
- in the definition of a partial specialization or of a member of a partial
  specialization:

A template argument is equivalent to a template parameter if

- for a type parameter, the template argument denotes the same type as the
  template parameter.
- for a non-type parameter, the template argument is an identifier that names a
  variable that is equivalent to the template parameter. A variable is
  equivalent to a template parameter if

```cpp
template<class T>
class A
{
    A* p1;      // A is the current instantiation
    A<T>* p2;   // A<T> is the current instantiation
    ::A<T>* p4; // ::A<T> is the current instantiation
    A<T*> p3;   // A<T*> is not the current instantiation

    class B
    {
        B* p1;                 // B is the current instantiation
        A<T>::B* p2;           // A<T>::B is the current instantiation
        typename A<T*>::B* p3; // A<T*>::B is not the current instantiation
    };
};

template<class T>
class A<T*>
{
    A<T*>* p1; // A<T*> is the current instantiation
    A<T>* p2;  // A<T> is not the current instantiation
};

template<int I>
struct B
{
    static const int my_I = I;
    static const int my_I2 = I + 0;
    static const int my_I3 = my_I;
    static const long my_I4 = I;
    static const int my_I5 = (I);

    B<my_I>* b1;  // B<my_I> is the current instantiation:
                  //   my_I has the same type as I,
                  //   and it is initialized with only I
    B<my_I2>* b2; // B<my_I2> is not the current instantiation:
                  //   I + 0 is not a single identifier
    B<my_I3>* b3; // B<my_I3> is the current instantiation:
                  //   my_I3 has the same type as I,
                  //   and it is initialized with only my_I (which is equivalent to I)
    B<my_I4>* b4; // B<my_I4> is not the current instantiation:
                  //   the type of my_I4 (long) is not the same as the type of I (int)
    B<my_I5>* b5; // B<my_I5> is not the current instantiation:
                  //   (I) is not a single identifier
};
```

Note that a base class can be the current instantiation if a nested class
derives from its enclosing class template. Base classes that are dependent types
but aren't the current instantiation are *dependent base classes*:

```cpp
template<class T>
struct A
{
    typedef int M;

    struct B
    {
        typedef void M;

        struct C;
    };
};

template<class T>
struct A<T>::B::C : A<T>
{
    M m; // OK, A<T>::M
};
```

A name is classified as a member of the current instantiation if it is

- an unqualified name that is found by unqualified lookup in the current
  instantiation or in its non-dependent base.
- qualified name, if the qualifier (the name to the left of `::`) names the
  current instantiation and lookup finds the name in the current instantiation
  or in its non-dependent base
- a name used in a class member access expression (`y` in `x.y` or `xp->y`),
  where the object expression (`x` or `*xp`) is the current instantiation and
  lookup finds the name in the current instantiation or in its non-dependent
  base

```cpp
template<class T>
class A
{
    static const int i = 5;

    int n1[i];       // i refers to a member of the current instantiation
    int n2[A::i];    // A::i refers to a member of the current instantiation
    int n3[A<T>::i]; // A<T>::i refers to a member of the current instantiation

    int f();
};

template<class T>
int A<T>::f()
{
    return i; // i refers to a member of the current instantiation
}
```

Members of the current instantiation may be both dependent and non-dependent.

If the lookup of a member of current instantiation gives a different result
between the point of instantiation and the point of definition, the lookup is
ambiguous. Note however that when a member name is used, it is not automatically
converted to a class member access expression, only explicit member access
expressions indicate members of current instantiation:

```cpp
struct A { int m; };
struct B { int m; };

template<typename T>
struct C : A, T
{
    int f() { return this->m; } // finds A::m in the template definition context
    int g() { return m; }       // finds A::m in the template definition context
};

template int C<B>::f(); // error: finds both A::m and B::m

template int C<B>::g(); // OK: transformation to class member access syntax
                        // does not occur in the template definition context
```

### Unknown specializations

Within a template definition, certain names are deduced to belong to an *unknown
specialization*, in particular,

- a qualified name, if any name that appears to the left of `::` is a *dependent
  type* that is not a member of the *current instantiation*
- a qualified name, whose qualifier is the *current instantiation*, and the name
  is not found in the current instantiation or any of its non-dependent base
  classes, and there is a dependent base class
- a name of a member in a class member access expression (the `y` in `x.y` or
  `xp->y`), if the type of the object expression (`x` or `*xp`) is a *dependent
  type* and is not the *current instantiation*
- a name of a member in a class member access expression (the `y` in `x.y` or
  `xp->y`), if the type of the object expression (`x` or `*xp`) is the *current
  instantiation*, and the name is not found in the current instantiation or any
  of its non-dependent base classes, and there is a dependent base class

```cpp
template<typename T>
struct Base {};

template<typename T>
struct Derived : Base<T>
{
    void f()
    {
        // Derived<T> refers to current instantiation
        // there is no 'unknown_type' in the current instantiation
        // but there is a dependent base (Base<T>)
        // Therefore, unknown_type is a member of unknown specialization
        typename Derived<T>::unknown_type z;
    }
};

template<>
struct Base<int> // this specialization provides it
{
    typedef int unknown_type;
};
```

This classification allows the following errors to be detected at the point of
template definition (rather than instantiation):

- If any template definition has a qualified name in which the qualifier refers
  to the *current instantiation* and the name is neither a member of *current
  instantiation* nor a member of *unknown specialization*, the program is
  ill-formed (no diagnostic required) even if the template is never
  instantiated.

```cpp
template<class T>
class A
{
    typedef int type;

    void f()
    {
        A<T>::type i; // OK: 'type' is a member of the current instantiation
        typename A<T>::other j; // Error:

        // 'other' is not a member of the current instantiation
        // and it is not a member of an unknown specialization
        // because A<T> (which names the current instantiation),
        // has no dependent bases for 'other' to hide in.
    }
};
```

- If any template definition has a member access expression where the object
  expression is the *current instantiation*, but the name is neither a member of
  *current instantiation* nor a member of *unknown specialization*, the program
  is ill-formed even if the template is never instantiated.

Members of unknown specialization are always dependent, and are looked up and
bound at the point of instantiation as all dependent names (see above)

### The typename disambiguator for dependent names

In a declaration or a definition of a template, including alias template, a name
that is not a member of the *current instantiation* and is dependent on a
template parameter is not considered to be a type unless the keyword `typename`
is used or unless it was already established as a type name, e.g. with a typedef
declaration or by being used to name a base class.

```cpp
#include <iostream>
#include <vector>

int p = 1;

template<typename T>
void foo(const std::vector<T> &v)
{
    // std::vector<T>::const_iterator is a dependent name,
    typename std::vector<T>::const_iterator it = v.begin();

    // without 'typename', the following is parsed as multiplication
    // of the type-dependent member variable 'const_iterator'
    // and some variable 'p'. Since there is a global 'p' visible
    // at this point, this template definition compiles.
    std::vector<T>::const_iterator* p;

    typedef typename std::vector<T>::const_iterator iter_t;
    iter_t * p2; // iter_t is a dependent name, but it's known to be a type name
}

template<typename T>
struct S
{
    typedef int value_t; // member of current instantiation

    void f()
    {
        S<T>::value_t n{}; // S<T> is dependent, but 'typename' not needed
        std::cout << n << '\n';
    }
};

int main()
{
    std::vector<int> v;
    foo(v); // template instantiation fails: there is no member variable
            // called 'const_iterator' in the type std::vector<int>
    S<int>().f();
}
```

The keyword `typename` may only be used in this way before qualified names (e.g.
`T::x`), but the names need not be dependent.

Usual qualified name lookup is used for the identifier prefixed by `typename`.
Unlike the case with elaborated type specifier, the lookup rules do not change
despite the qualifier:

```cpp
struct A // A has a nested variable X and a nested type struct X
{
    struct X {};
    int X;
};

struct B
{
    struct X {}; // B has a nested type struct X
};

template<class T>
void f(T t)
{
    typename T::X x;
}

void foo()
{
    A a;
    B b;
    f(b); // OK: instantiates f<B>, T::X refers to B::X
    f(a); // error: cannot instantiate f<A>:
          // because qualified name lookup for A::X finds the data member
}
```

The keyword `typename` can be used even outside of templates.

```cpp
#include <vector>

int main()
{
    // Both OK (after resolving CWG 382)
    typedef typename std::vector<int>::const_iterator iter_t;
    typename std::vector<int> v;
}
```

In some contexts, only type names can validly appear. In these contexts, a
dependent qualified name is assumed to name a type and no `typename` is
required:
- A qualified name that is used as a declaration specifier in the (top-level)
  decl-specifier-seq of:
- A qualified name that appears in *type-id*, where the smallest enclosing
  *type-id* is:
*(since C++20)*

### The template disambiguator for dependent names

Similarly, in a template definition, a dependent name that is not a member of
the *current instantiation* is not considered to be a template name unless the
disambiguation keyword `template` is used or unless it was already established
as a template name:

```cpp
template<typename T>
struct S
{
    template<typename U>
    void foo() {}
};

template<typename T>
void bar()
{
    S<T> s;
    s.foo<T>();          // error: < parsed as less than operator
    s.template foo<T>(); // OK
}
```

The keyword `template` may only be used in this way after operators `::` (scope
resolution), `->` (member access through pointer), and `.` (member access), the
following are all valid examples:

- `T::template foo<X>();`
- `s.template foo<X>();`
- `this->template foo<X>();`
- `typename T::template iterator<int>::value_type v;`

As is the case with `typename`, the `template` prefix is allowed even if the
name is not dependent or the use does not appear in the scope of a template.

Even if the name to the left of `::` refers to a namespace, the template
disambiguator is allowed:

```cpp
template<typename>
struct S {};

::template S<void> q; // allowed, but unnecessary
```

Due to the special rules for unqualified name lookup for template names in
member access expressions, when a non-dependent template name appears in a
member access expression (after `->` or after `.`), the disambiguator is
unnecessary if there is a class or alias(since C++11) template with the same
name found by ordinary lookup in the context of the expression. However, if the
template found by lookup in the context of the expression differs from the one
found in the context of the class, the program is ill-formed(until C++11)
```cpp
template<int>
struct A { int value; };
template<class T>
void f(T t)
{
    t.A<0>::value; // Ordinary lookup of A finds a class template.
                   // A<0>::value names member of class A<0>
    // t.A < 0;    // Error: '<' is treated as the start of template argument list
}
```
*(until C++23)*

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 206 | C++98 | it was unspecified at what point semantic constraints are
      applied when a type used in a non-dependent name is incomplete at the
      point at which a template is defined but is complete at the point at which
      an instantiation is performed | the program is ill-formed and no
      diagnostic is required in this case
  CWG 224 | C++98 | the definition of dependent types was based on the form of
      the name rather than lookup | definition revamped
  CWG 382 | C++98 | the `typename` disambiguator was only allowed in template
      scope | also allowed outside of templates
  CWG 468 | C++98 | the `template` disambiguator was only allowed in template
      scope | also allowed outside of templates
  CWG 502 | C++98 | it was unspecified whether nested enumerations are dependent
      | dependent as nested classes
  CWG 1047 | C++98 | `typeid` expressions were never value-dependent |
      value-dependent if the operand is type-dependent
  CWG 1160 | C++98 | it was unspecified whether a name refers to the current
      instantiation when a template-id matching a primary template or partial
      specialization appears in the definition of a member of the template |
      specified
  CWG 1413 | C++98 | uninitialized static data member, static member function,
      and address of member of a class template weren't listed as
      value-dependent | listed
  CWG 1471 | C++98 | a nested type of a non-dependent base of the current
      instantiation was dependent | it is not dependent
  CWG 1850 | C++98 | the list of cases that meaning may change between the
      definition context and the point of instantiation was incomplete | made
      complete
  CWG 1929 | C++98 | it was not clear whether the `template` disambiguator can
      follow a `::` where the name to its left refers to a namespace | allowed
  CWG 2100 | C++98 | address of a static data member of class template wasn't
      listed as value-dependent | listed
  CWG 2276 | C++98 | a function type whose exception specification is
      value-dependent was not a dependent type | it is
  CWG 2307 | C++98 | a parenthesized non-type template parameter used as a
      template argument was equivalent to that template parameter | not
      equivalent anymore
  CWG 2457 | C++11 | a function type with function parameter pack was not a
      dependent type | it is

---
*Source: https://en.cppreference.com/w/cpp/language/dependent_name*
