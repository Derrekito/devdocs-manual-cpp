# Function declaration

A function declaration introduces the function name and its type. A function
definition associates the function name/type with the function body.

### Function declaration

Function declarations may appear in any scope. A function declaration at class
scope introduces a class member function (unless the friend specifier is used),
see member functions and friend functions for details.

```text
noptr-declarator ( parameter-list ) cv ﻿(optional) ref ﻿ ﻿(optional) except ﻿(optional) attr ﻿(optional)    (1)
noptr-declarator ( parameter-list ) cv ﻿(optional) ref ﻿ ﻿(optional) except ﻿(optional) attr ﻿(optional)-> trailing    (2)    (since C++11)
```

(see Declarations for the other forms of the declarator syntax)

1) Regular function declarator syntax.

2) Trailing return type declaration. The decl-specifier-seq in this case must
   contain the keyword `auto`.

- **noptr-declarator** — any valid declarator, but if it begins with `*`, `&`,
  or `&&`, it has to be surrounded by parentheses.
- **parameter-list** — possibly empty, comma-separated list of the function
  parameters (see below for details)
- **attr** — (since C++11) a list of attributes. These attributes are applied to
  the type of the function, not the function itself. The attributes for the
  function appear after the identifier within the declarator and are combined
  with the attributes that appear in the beginning of the declaration, if any.
- **cv** — const/volatile qualification, only allowed in non-static member
  function declarations
- **ref** — (since C++11) ref-qualification, only allowed in non-static member
  function declarations
- **except** — dynamic exception specification (until C++11) either dynamic
  exception specification or noexcept specification (since C++11) (until C++17)
  noexcept specification (since C++17) Note that the exception specification is
  not part of the function type (until C++17)
- **dynamic exception specification** — (until C++11)
- **either dynamic exception specification or noexcept specification** — (since
  C++11) (until C++17)
- **noexcept specification** — (since C++17)
- **Note that the exception specification is not part of the function type** —
  (until C++17)
- **trailing** — Trailing return type, useful if the return type depends on
  argument names, such as `template<class T, class U> auto add(T t, U u) ->
  decltype(t + u);` or is complicated, such as in `auto fpif(int)->int(*)(int)`

As mentioned in Declarations, the declarator can be followed by a *requires*
clause, which declares the associated constraints for the function, which must
be satisfied in order for the function to be selected by overload resolution.
(example: `void f1(int a) requires true;`) Note that the associated constraint
is part of function signature, but not part of function type.
*(since C++20)*

Function declarators can be mixed with other declarators, where
decl-specifier-seq allows:

```cpp
// declares an int, an int*, a function, and a pointer to a function
int a = 1, *p = NULL, f(), (*pf)(double);
// decl-specifier-seq is int
// declarator f() declares (but doesn't define)
//                a function taking no arguments and returning int

struct S
{
    virtual int f(char) const, g(int) &&; // declares two non-static member functions
    virtual int f(char), x; // compile-time error: virtual (in decl-specifier-seq)
                            // is only allowed in declarations of non-static
                            // member functions
};
```

Using a volatile-qualified object type as parameter type or return type is
deprecated.
*(since C++20)*

The return type of a function cannot be a function type or an array type (but
can be a pointer or reference to those).

As with any declaration, attributes that appear before the declaration and the
attributes that appear immediately after the identifier within the declarator
both apply to the entity being declared or defined (in this case, to the
function):
```cpp
[[noreturn]] void f [[noreturn]] (); // okay: both attributes apply to the function f
```
However, the attributes that appear after the declarator (in the syntax above),
apply to the type of the function, not to the function itself:
```cpp
void f() [[noreturn]]; // error: this attribute has no effect on the function itself
```
*(since C++11)*

### Return type deduction
If the decl-specifier-seq of the function declaration contains the keyword auto,
trailing return type may be omitted, and will be deduced by the compiler from
the type of the expression used in the return statement. If the return type does
not use decltype(auto), the deduction follows the rules of template argument
deduction:
```cpp
int x = 1;
auto f() { return x; }        // return type is int
const auto& f() { return x; } // return type is const int&
```
If the return type is decltype(auto), the return type is as what would be
obtained if the expression used in the return statement were wrapped in
`decltype`:
```cpp
int x = 1;
decltype(auto) f() { return x; }  // return type is int, same as decltype(x)
decltype(auto) f() { return(x); } // return type is int&, same as decltype((x))
```
(note: "const decltype(auto)&" is an error, decltype(auto) must be used on its
own)
If there are multiple return statements, they must all deduce to the same type:
```cpp
auto f(bool val)
{
    if (val) return 123; // deduces return type int
    else return 3.14f;   // error: deduces return type float
}
```
If there is no return statement or if the argument of the return statement is a
void expression, the declared return type must be either decltype(auto), in
which case the deduced return type is void, or (possibly cv-qualified) auto, in
which case the deduced return type is then (identically cv-qualified) void:
```cpp
auto f() {}              // returns void
auto g() { return f(); } // returns void
auto* x() {}             // error: cannot deduce auto* from void
```
Once a return statement has been seen in a function, the return type deduced
from that statement can be used in the rest of the function, including in other
return statements:
```cpp
auto sum(int i)
{
    if (i == 1)
        return i;              // sum’s return type is int
    else
        return sum(i - 1) + i; // okay: sum’s return type is already known
}
```
If the return statement uses a braced-init-list, deduction is not allowed:
```cpp
auto func() { return {1, 2, 3}; } // error
```
Virtual functions and coroutines(since C++20) cannot use return type deduction:
```cpp
struct F
{
    virtual auto f() { return 2; } // error
};
```
Function templates other than user-defined conversion functions can use return
type deduction. The deduction takes place at instantiation even if the
expression in the return statement is not dependent. This instantiation is not
in an immediate context for the purposes of SFINAE.
```cpp
template<class T>
auto f(T t) { return t; }
typedef decltype(f(1)) fint_t;    // instantiates f<int> to deduce return type
template<class T>
auto f(T* t) { return *t; }
void g() { int (*p)(int*) = &f; } // instantiates both fs to determine return types,
                                  // chooses second template overload
```
Redeclarations or specializations of functions or function templates that use
return type deduction must use the same return type placeholders:
```cpp
auto f(int num) { return num; }
// int f(int num);            // error: no placeholder return type
// decltype(auto) f(int num); // error: different placeholder
template<typename T>
auto g(T t) { return t; }
template auto g(int);     // okay: return type is int
// template char g(char); // error: not a specialization of the primary template g
```
Similarly, redeclarations or specializations of functions or function templates
that do not use return type deduction must not use a placeholder:
```cpp
int f(int num);
// auto f(int num) { return num; } // error: not a redeclaration of f
template<typename T>
T g(T t) { return t; }
template int g(int);      // okay: specialize T as int
// template auto g(char); // error: not a specialization of the primary template g
```
Explicit instantiation declarations do not themselves instantiate function
templates that use return type deduction:
```cpp
template<typename T>
auto f(T t) { return t; }
extern template auto f(int); // does not instantiate f<int>
int (*p)(int) = f; // instantiates f<int> to determine its return type,
                   // but an explicit instantiation definition
                   // is still required somewhere in the program
```
*(since C++14)*

### Parameter list

Parameter list determines the arguments that can be specified when the function
is called. It is a comma-separated list of *parameter declarations*, each of
which has the following syntax:

```text
attr ﻿(optional) decl-specifier-seq declarator    (1)
attr ﻿(optional) decl-specifier-seq declarator = initializer    (2)
attr ﻿(optional) decl-specifier-seq abstract-declarator ﻿(optional)    (3)
attr ﻿(optional) decl-specifier-seq abstract-declarator ﻿(optional) = initializer    (4)
void    (5)
```

1) Declares a named (formal) parameter. For the meanings of decl-specifier-seq
   and declarator, see declarations.

`int f(int a, int* p, int (*(*x)(double))[3]);`

2) Declares a named (formal) parameter with a default value.

`int f(int a = 7, int* p = nullptr, int (*(*x)(double))[3] = nullptr);`

3) Declares an unnamed parameter.

`int f(int, int*, int (*(*)(double))[3]);`

4) Declares an unnamed parameter with a default value.

`int f(int = 7, int* = nullptr, int (*(*)(double))[3] = nullptr);`

5) Indicates that the function takes no parameters, it is the exact synonym for
   an empty parameter list: `int f(void);` and `int f();` declare the same
   function. Note that the type `void` (possibly cv-qualified) cannot be used in
   a parameter list otherwise: `int f(void, int);` and `int f(const void);` are
   errors (although derived types, such as `void*` can be used). In a template,
   only non-dependent void type can be used (a function taking a single
   parameter of type `T` does not become a no-parameter function if instantiated
   with `T = void`).

An ellipsis `...` may appear at the end of the parameter list; this declares a
variadic function:

```cpp
int printf(const char* fmt ...);
```

For compatibility with C89, an optional comma may appear before the ellipsis if
the parameter list contains at least one parameter:

```cpp
int printf(const char* fmt, ...); // OK, same as above
```

Although decl-specifier-seq implies there can exist specifiers other than type
specifiers, the only other specifier allowed is register as well as `auto`(until
C++11), and it has no effect.
*(until C++17)*

If any of the function parameters uses a *placeholder* (either auto or a concept
type), the function declaration is instead an abbreviated function template
declaration:
```cpp
void f1(auto);    // same as template<class T> void f1(T)
void f2(C1 auto); // same as template<C1 T> void f2(T), if C1 is a concept
```
*(since C++20)*

Parameter names declared in function declarations are usually for only
self-documenting purposes. They are used (but remain optional) in function
definitions.

The type of each function parameter in the parameter list is determined
according to the following rules:

1) First, decl-specifier-seq and the declarator are combined as in any
   declaration to determine the type.

2) If the type is "array of T" or "array of unknown bound of T", it is replaced
   by the type "pointer to T".

3) If the type is a function type F, it is replaced by the type "pointer to F".

4) Top-level cv-qualifiers are dropped from the parameter type (This adjustment
   only affects the function type, but doesn't modify the property of the
   parameter: `int f(const int p, decltype(p)*);` and `int f(int, const int*);`
   declare the same function).

Because of these rules, the following function declarations declare exactly the
same function:

```cpp
int f(char s[3]);
int f(char[]);
int f(char* s);
int f(char* const);
int f(char* volatile s);
```

The following declarations also declare exactly the same function:

```cpp
int f(int());
int f(int (*g)());
```

An ambiguity arises in a parameter list when a type name is nested in
parentheses (including lambda expressions)(since C++11). In this case, the
choice is between the declaration of a parameter of type pointer to function and
the declaration of a parameter with redundant parentheses around the identifier
of the declarator. The resolution is to consider the type name as a simple type
specifier (which is the pointer to function type):

```cpp
class C {};

void f(int(C)) {} // void f(int(*fp)(C param)) {}
                  // NOT void f(int C) {}

void g(int *(C[10])); // void g(int *(*fp)(C param[10]));
                      // NOT void g(int *C[10]);
```

Parameter type cannot be a type that includes a reference or a pointer to array
of unknown bound, including a multi-level pointers/arrays of such types, or a
pointer to functions whose parameters are such types.

The ellipsis that indicates variadic arguments need not be preceded by a comma,
even if it follows the ellipsis that indicates a parameter pack expansion, so
the following function templates are exactly the same:
```cpp
template<typename... Args>
void f(Args..., ...);
template<typename... Args>
void f(Args... ...);
template<typename... Args>
void f(Args......);
```
An example of when such declaration might be used is the possible implementation
of `std::is_function`.
```cpp
#include <cstdio>
template<typename... Variadic, typename... Args>
constexpr void invoke(auto (*fun)(Variadic......), Args... args)
{
    fun(args...);
}
int main()
{
    invoke(std::printf, "%dm•%dm•%dm = %d%s%c", 2,3,7, 2*3*7, "m³", '\n');
}
```
Output:
```text
2m•3m•7m = 42m³
```
*(since C++11)*

### Function type

#### Parameter-type-list

A function’s *parameter-type-list* is determined as follows:

1. The type of each parameter (including function parameter packs)(since C++11)
   is determined from its own parameter declaration.
1. After determining the type of each parameter, any parameter of type “array of
   `T`” or of function type `T` is adjusted to be “pointer to `T`”.
1. After producing the list of parameter types, any top-level cv-qualifiers
   modifying a parameter type are deleted when forming the function type.
1. The resulting list of transformed parameter types and the presence or absence
   of the ellipsis or a function parameter pack(since C++11) is the function’s
   parameter-type-list.

```cpp
void f(char*);         // #1
void f(char[]) {}      // defines #1
void f(const char*) {} // OK, another overload
void f(char* const) {} // error: redefines #1

void g(char(*)[2]);   // #2
void g(char[3][2]) {} // defines #2
void g(char[3][3]) {} // OK, another overload

void h(int x(const int)); // #3
void h(int (*)(int)) {}   // defines #3
```

#### Determining function type

In syntax (1), assuming noptr-declarator as a standalone declaration, given the
type of the qualified-id or unqualified-id in noptr-declarator as
“derived-declarator-type-list `T`”:

- If the exception specification is non-throwing, the type of the function
  declared is “derived-declarator-type-list noexcept function of
  parameter-type-list cv ﻿(optional) ref ﻿ ﻿(optional) returning `T`”.
*(since C++17)*

- The(until C++17)Otherwise, the(since C++17) type of the function declared is
  “derived-declarator-type-list function of parameter-type-list cv ﻿(optional)
  ref ﻿ ﻿(optional)(since C++11) returning `T`”.

In syntax (2), assuming noptr-declarator as a standalone declaration, given the
type of the qualified-id or unqualified-id in noptr-declarator as
“derived-declarator-type-list `T`” (`T` must be auto in this case):
- If the exception specification is non-throwing, the type of the function
  declared is “derived-declarator-type-list noexcept function of
  parameter-type-list cv ﻿(optional) ref ﻿ ﻿(optional) returning trailing ﻿”.
*(since C++17)*
- The(until C++17)Otherwise, the(since C++17) type of the function declared is
  “derived-declarator-type-list function of parameter-type-list cv ﻿(optional)
  ref ﻿ ﻿(optional) returning trailing ﻿”.
*(since C++11)*

- If the exception specification is non-throwing, the type of the function
  declared is “derived-declarator-type-list noexcept function of
  parameter-type-list cv ﻿(optional) ref ﻿ ﻿(optional) returning trailing ﻿”.
*(since C++17)*

attr, if present, applies to the function type.
*(since C++11)*

```cpp
// the type of “f1” is
// “function of int returning void, with attribute noreturn”
void f1(int a) [[noreturn]];

// the type of “f2” is
// “constexpr noexcept function of pointer to int returning int”
constexpr auto f2(int[] b) noexcept -> int;

struct X
{
    // the type of “f3” is
    // “function of no parameter const returning const int”
    const int f3() const;
};
```

#### Trailing qualifiers

A function type with cv ﻿ or ref ﻿ ﻿(since C++11) (including a type named by
`typedef` name) can appear only as:

- the function type for a non-static member function,
- the function type to which a pointer to member refers,
- the top-level function type of a function typedef declaration or alias
  declaration(since C++11),
- the type-id in the default argument of a template type parameter, or
- the type-id of a template argument for a template type parameter.

```cpp
typedef int FIC(int) const;
FIC f;     // Error: does not declare a member function

struct S
{
    FIC f; // OK
};

FIC S::*pm = &S::f; // OK
```

### Function definition

A non-member function definition may appear at namespace scope only (there are
no nested functions). A member function definition may also appear in the body
of a class definition. They have the following syntax:

```text
attr ﻿(optional) decl-specifier-seq ﻿(optional) declarator virt-specifier-seq ﻿(optional) function-body
```

where function-body is one of the following

```text
ctor-initializer ﻿(optional) compound-statement    (1)
function-try-block    (2)
= delete ;    (3)    (since C++11)
= default ;    (4)    (since C++11)
```

1) Regular function body.

2) Function-try-block (which is a regular function body wrapped in a try/catch
   block).

3) Explicitly deleted function definition.

4) Explicitly defaulted function definition, only allowed for special member
   functions and comparison operator functions(since C++20).

- **attr** — (since C++11) a list of attributes. These attributes are combined
  with the attributes after the identifier in the declarator (see top of this
  page), if any.
- **decl-specifier-seq** — the return type with specifiers, as in the
  declaration grammar
- **declarator** — function declarator, same as in the function declaration
  grammar above (can be parenthesized). as with function declaration, it may be
  followed by a requires-clause(since C++20)
- **virt-specifier-seq** — (since C++11) `override`, `final`, or their
  combination in any order (only allowed for non-static member functions)
- **ctor-initializer** — member initializer list, only allowed in constructors
- **compound-statement** — the brace-enclosed sequence of statements that
  constitutes the body of a function

```cpp
int max(int a, int b, int c)
{
    int m = (a > b) ? a : b;
    return (m > c) ? m : c;
}

// decl-specifier-seq is "int"
// declarator is "max(int a, int b, int c)"
// body is { ... }
```

The function body is a compound statement (sequence of zero or more statements
surrounded by a pair of curly braces), which is executed when the function call
is made.

The parameter types, as well as the return type of a function definition cannot
be (possibly cv-qualified) incomplete class types unless the function is defined
as deleted(since C++11). The completeness check is only made in the function
body, which allows member functions to return the class in which they are
defined (or its enclosing class), even if it is incomplete at the point of
definition (it is complete in the function body).

The parameters declared in the declarator of a function definition are in scope
within the body. If a parameter is not used in the function body, it does not
need to be named (it's sufficient to use an abstract declarator):

```cpp
void print(int a, int) // second parameter is not used
{
    std::printf("a = %d\n", a);
}
```

Even though top-level cv-qualifiers on the parameters are discarded in function
declarations, they modify the type of the parameter as visible in the body of a
function:

```cpp
void f(const int n) // declares function of type void(int)
{
    // but in the body, the type of n is const int
}
```

### Deleted functions
If, instead of a function body, the special syntax `= delete;` is used, the
function is defined as *explicitly deleted*. Any use of a deleted function is
ill-formed (the program will not compile). This includes calls, both explicit
(with a function call operator) and implicit (a call to deleted overloaded
operator, special member function, allocation function etc), constructing a
pointer or pointer-to-member to a deleted function, and even the use of a
deleted function in an expression that is not potentially-evaluated. However,
implicit ODR-use of a non-pure virtual member function that happens to be
deleted is allowed.
If the function is overloaded, overload resolution takes place first, and the
program is only ill-formed if the deleted function was selected:
```cpp
struct sometype
{
    void* operator new(std::size_t) = delete;
    void* operator new[](std::size_t) = delete;
};
sometype* p = new sometype; // error: attempts to call deleted sometype::operator new
```
The deleted definition of a function must be the first declaration in a
translation unit: a previously-declared function cannot be redeclared as
deleted:
```cpp
struct sometype { sometype(); };
sometype::sometype() = delete; // error: must be deleted on the first declaration
```
### User-provided functions
A function is *user-provided* if it is user-declared and not explicitly
defaulted or deleted on its first declaration. A user-provided
explicitly-defaulted function (i.e., explicitly defaulted after its first
declaration) is defined at the point where it is explicitly defaulted; if such a
function is implicitly defined as deleted, the program is ill-formed. Declaring
a function as defaulted after its first declaration can provide efficient
execution and concise definition while enabling a stable binary interface to an
evolving code base.
```cpp
// All special member functions of “trivial” are
// defaulted on their first declarations respectively,
// they are not user-provided
struct trivial
{
    trivial() = default;
    trivial(const trivial&) = default;
    trivial(trivial&&) = default;
    trivial& operator=(const trivial&) = default;
    trivial& operator=(trivial&&) = default;
    ~trivial() = default;
};
struct nontrivial
{
    nontrivial(); // first declaration
};
// not defaulted on the first declaration,
// it is user-provided and is defined here
nontrivial::nontrivial() = default;
```
### __func__
Within the function body, the function-local predefined variable `__func__` is
defined as if by
```cpp
static const char __func__[] = "function-name";
```
This variable has block scope and static storage duration:
```cpp
struct S
{
    S(): s(__func__) {} // okay: initializer-list is part of function body
    const char* s;
};
void f(const char* s = __func__); // error: parameter-list is part of declarator
```
```cpp
#include <iostream>
void Foo() { std::cout << __func__ << ' '; }
struct Bar
{
    Bar() { std::cout << __func__ << ' '; }
    ~Bar() { std::cout << __func__ << ' '; }
    struct Pub { Pub() { std::cout << __func__ << ' '; } };
};
int main()
{
    Foo();
    Bar bar;
    Bar::Pub pub;
}
```
Possible output:
```text
Foo Bar Pub ~Bar
```
*(since C++11)*

### Notes

In case of ambiguity between a variable declaration using the
direct-initialization syntax and a function declaration, the compiler always
chooses function declaration; see direct-initialization.

  Feature-test macro | Value | Std | Feature
  `__cpp_decltype_auto` | 201304L | (C++14) | `decltype(auto)`
  `__cpp_return_type_deduction` | 201304L | (C++14) | Return type deduction for
      normal functions

### Example

```cpp
#include <iostream>
#include <string>

// simple function with a default argument, returning nothing
void f0(const std::string& arg = "world!")
{
    std::cout << "Hello, " << arg << '\n';
}

// the declaration is in namespace (file) scope
// (the definition is provided later)
int f1();

// function returning a pointer to f0, pre-C++11 style
void (*fp03())(const std::string&)
{
    return f0;
}

// function returning a pointer to f0, with C++11 trailing return type
auto fp11() -> void(*)(const std::string&)
{
    return f0;
}

int main()
{
    f0();
    fp03()("test!");
    fp11()("again!");
    int f2(std::string) noexcept; // declaration in function scope
    std::cout << "f2(\"bad\"): " << f2("bad") << '\n';
    std::cout << "f2(\"42\"): " << f2("42") << '\n';
}

// simple non-member function returning int
int f1()
{
    return 007;
}

// function with an exception specification and a function try block
int f2(std::string str) noexcept
try
{
    return std::stoi(str);
}
catch (const std::exception& e)
{
    std::cerr << "stoi() failed!\n";
    return 0;
}
```

Possible output:

```text
stoi() failed!
Hello, world!
Hello, test!
Hello, again!
f2("bad"): 0
f2("42"): 42
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 135 | C++98 | member functions defined in class could not have a parameter
      of or return its own class because it is incomplete | allowed
  CWG 332 | C++98 | a parameter could have cv-qualified void type | prohibited
  CWG 393 | C++98 | types that include pointers/references to array of unknown
      bound could not be parameters | such types are allowed
  CWG 452 | C++98 | member initializer list was not a part of function body |
      made it a part of function body by modifying the syntax of function
      definition
  CWG 577 | C++98 | dependent type void could be used to declare a function
      taking no parameters | only non-dependent void is allowed
  CWG 1327 | C++11 | defaulted or deleted functions could not be specified with
      override or final | allowed
  CWG 1355 | C++11 | only special member functions could be user-provided |
      extended to all functions
  CWG 1394 | C++11 | deleted functions could not have any parameter of an
      incomplete type or return an incomplete type | incomplete type allowed
  CWG 1824 | C++98 | the completeness check on parameter type and return type of
      a function definition could be made outside the context of the function
      definition | only make the completeness check in the context of the
      function definition
  CWG 1877 | C++14 | return type deduction treated `return;` as `return void();`
      | simply deduce the return type as void in this case
  CWG 2015 | C++11 | the implicit odr-use of a deleted virtual function was
      ill-formed | such odr-uses are exempt from the use prohibition
  CWG 2044 | C++14 | return type deduction on functions returning void would
      fail if the declared return type is decltype(auto) | updated the deduction
      rule to handle this case
  CWG 2081 | C++14 | function redeclarations could use return type deduction
      even if the initial declaration does not | not allowed
  CWG 2145 | C++98 | the declarator in function definition could not be
      parenthesized | allowed
  CWG 2259 | C++11 | the ambiguity resolution rule regarding parenthesized type
      names did not cover lambda expressions | covered
  CWG 2430 | C++98 | in the definition of a member function in a class
      definition, the type of that class could not be the return type or
      parameter type due to the resolution of CWG issue 1824 | only make the
      completeness check in the function body

### See also

**C documentation for Declaring functions**

---
*Source: https://en.cppreference.com/w/cpp/language/function*
