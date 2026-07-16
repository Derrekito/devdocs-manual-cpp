# Language linkage

Provides for linkage between program units written in different programming
languages.

```text
extern string-literal { declaration-seq ﻿(optional) }    (1)
extern string-literal declaration    (2)
```

1) Applies the language specification string-literal to all function types,
   function names with external linkage and variables with external linkage
   declared in declaration-seq.

2) Applies the language specification string-literal to a single declaration or
   definition.

- **string-literal** — an unevaluated string literal that names the required
  language linkage
- **declaration-seq** — a sequence of declarations, which may include nested
  linkage specifications
- **declaration** — a declaration

### Explanation

Every function type, every function name with external linkage, and every
variable name with external linkage, has a property called *language linkage*.
Language linkage encapsulates the set of requirements necessary to link with a
program unit written in another programming language: calling convention, name
mangling (name decoration) algorithm, etc.

Only two language linkages are guaranteed to be supported:

1. "C++", the default language linkage.
1. "C", which makes it possible to link with functions written in the C
   programming language, and to define, in a C++ program, functions that can be
   called from the units written in C.

```cpp
extern "C"
{
    int open(const char *path_name, int flags); // C function declaration
}

int main()
{
    int fd = open("test.txt", 0); // calls a C function from a C++ program
}

// This C++ function can be called from C code
extern "C" void handler(int)
{
    std::cout << "Callback invoked\n"; // It can use C++
}
```

Since language linkage is part of every function type, pointers to functions
maintain language linkage as well. Language linkage of function types (which
represents calling convention) and language linkage of function names (which
represents name mangling) are independent of each other:

```cpp
extern "C" void f1(void(*pf)()); // declares a function f1 with C linkage,
                             // which returns void and takes a pointer to a C function
                             // which returns void and takes no parameters

extern "C" typedef void FUNC(); // declares FUNC as a C function type that returns void
                                // and takes no parameters

FUNC f2;            // the name f2 has C++ linkage, but its type is C function
extern "C" FUNC f3; // the name f3 has C linkage and its type is C function void()
void (*pf2)(FUNC*); // the name pf2 has C++ linkage, and its type is
                    // "pointer to a C++ function which returns void and takes one
                    // argument of type 'pointer to the C function which returns void
                    // and takes no parameters'"

extern "C"
{
    static void f4(); // the name of the function f4 has internal linkage (no language)
                      // but the function's type has C language linkage
}
```

Two functions with the same name and the same parameter list in the same
namespace cannot have two different language linkages (note, however, that
linkage of a parameter may permit such overloading, as in the case of
`std::qsort` and `std::bsearch`). Likewise, two variables in the same namespace
cannot have two different language linkages.

#### Special rules for "C" linkage

When class members, friend functions with a trailing requires-clause,(since
C++20) or non-static member functions appear in a "C" language block, the
linkage of their types remains "C++" (but parameter types, if any, remain "C"):

```cpp
extern "C"
{
    class X
    {
        void mf();           // the function mf and its type have C++ language linkage
        void mf2(void(*)()); // the function mf2 has C++ language linkage;
                             // the parameter has type “pointer to C function”
    };
}
```

When a function or a variable is declared (in any namespace) with "C" language
linkage, all declarations of functions (in any namespace) and all declarations
of variables in global scope with the same unqualified name must refer to the
same function or variable.

```cpp
int x;

namespace A
{
    extern "C" int x(); // error: same name as global-namespace variable x
}
```

```cpp
namespace A
{
    extern "C" int f();
}

namespace B
{
    extern "C" int f();   // A::f and B::f refer to the same function f with C linkage
}

int A::f() { return 98; } // definition for that function
```

```cpp
namespace A
{
    extern "C" int g() { return 1; }
}

namespace B
{
    extern "C" int g() { return 1; } // error: redefinition of the same function
}
```

```cpp
namespace A
{
    extern "C" int h();
}

extern "C" int h() { return 97; } // definition for the C linkage function h
                                  // A::h and ::h refer to the same function
```

Note: the special rule that excludes friend with trailing requires clauses makes
it possible to declare a friend function with the same name as a global scope
"C" function using a dummy clause:
```cpp
template<typename T>
struct A { struct B; };
extern "C"
{
    template<typename T>
    struct A<T>::B
    {
        friend void f(B*) requires true {} // C language linkage ignored
    };
}
namespace Q
{
    extern "C" void f(); // not ill-formed
}
```
*(since C++23)*

### Notes

Language specifications can only appear in namespace scope.

The braces of the language specification do not establish a scope.

When language specifications nest, the innermost specification is the one that
is in effect.

A function can be re-declared without a linkage specification after it was
declared with a language specification, the second declaration will reuse the
first language linkage. The opposite is not true: if the first declaration has
no language linkage, it is assumed "C++", and redeclaring with another language
is an error.

A declaration directly contained in a language linkage specification is treated
as if it contains the `extern` specifier for the purpose of determining the
linkage of the declared name and whether it is a definition.

```cpp
extern "C" int x; // a declaration and not a definition
// The above line is equivalent to extern "C" { extern int x; }

extern "C" { int x; } // a declaration and definition

extern "C" double f();
static double f(); // error: linkage conflict

extern "C" static void g(); // error: linkage conflict
```

extern "C" makes it possible to include header files containing declarations of
C library functions in a C++ program, but if the same header file is shared with
a C program, extern "C" (which is not allowed in C) must be hidden with an
appropriate `#ifdef`, typically `__cplusplus`:

```cpp
#ifdef __cplusplus
extern "C" int foo(int, int); // C++ compiler sees this
#else
int foo(int, int);            // C compiler sees this
#endif
```

The only modern compiler that differentiates function types with "C" and "C++"
language linkages is Oracle Studio, others do not permit overloads that are only
different in language linkage, including the overload sets required by the C++
standard (`std::qsort`, `std::bsearch`, `std::signal`, `std::atexit`, and
`std::at_quick_exit`): GCC bug 2316, Clang bug 6277, CWG issue 1555.

```cpp
extern "C"   using c_predfun   = int(const void*, const void*);
extern "C++" using cpp_predfun = int(const void*, const void*);

// ill-formed, but accepted by most compilers
static_assert(std::is_same<c_predfun, cpp_predfun>::value,
              "C and C++ language linkages shall not differentiate function types.");

// following declarations do not declare overloads in most compilers
// because c_predfun and cpp_predfun are considered to be the same type
void qsort(void* base, std::size_t nmemb, std::size_t size, c_predfun*   compar);
void qsort(void* base, std::size_t nmemb, std::size_t size, cpp_predfun* compar);
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 4 | C++98 | names with internal linkage can have language linkages |
      limited to names with external linkage
  CWG 341 | C++98 | a function with "C" language linkage can have the same name
      as a global variable | the program is ill-formed in this case (no
      diagnostic required if they appear in different translation units)
  CWG 564 | C++98 | the program was ill-formed if two declarations only differ
      in language linkage specifications (i.e. different string literals
      following 'extern') | the actual language linkages given by the
      declarations are compared instead
  CWG 2460 | C++20 | friend functions with a trailing requires-clause and "C"
      language linkage had conflict behaviors | "C" language linkage is ignored
      in this case
  CWG 2483 | C++98 | the linkage of the types of static member functions appear
      in "C" language blocks was "C++" | the linkage is "C"

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++03 standard (ISO/IEC 14882:2003):

---
*Source: https://en.cppreference.com/w/cpp/language/language_linkage*
