# typedef specifier

- `typedef` - creates an alias that can be used anywhere in place of a (possibly
  complex) type name.

### Explanation

The typedef specifier, when used in a declaration, specifies that the
declaration is a *typedef declaration* rather than a variable or function
declaration.

Typically, the typedef specifier appears at the start of the declaration, though
it is permitted to appear after the type specifiers, or between two type
specifiers. The typedef specifier cannot be combined with any other specifier
except for type specifiers.

A typedef declaration may declare one or many identifiers on the same line (e.g.
int and a pointer to int), it may declare array and function types, pointers and
references, class types, etc. Every identifier introduced in this declaration
becomes a *typedef name*, which is a synonym for the type of the object or
function that it would become if the keyword typedef were removed.

The typedef names are aliases for existing types, and are not declarations of
new types. typedef cannot be used to change the meaning of an existing type name
(including a typedef name). Once declared, a typedef name may only be redeclared
to refer to the same type again. Typedef names are only in effect in the scope
where they are visible: different functions or class declarations may define
identically-named types with different meaning.

The typedef specifier may not appear in the declaration of a function parameter
nor in the decl-specifier-seq of a function definition:

```cpp
void f1(typedef int param); // ill-formed
typedef int f2() {}         // ill-formed
```

The typedef specifier may not appear in a declaration that does not contain a
declarator:

```cpp
typedef struct X {}; // ill-formed
```

### typedef name for linkage purposes

If a typedef declaration defines an unnamed class or enumeration, the first
typedef name of the class type or enumeration type declared by the declaration
is the *typedef name for linkage purposes* of that type.

For example, in `typedef struct { /* ... */ } S;`, `S` is a typedef name for
linkage purposes. The class or enumeration type defined in this way has external
linkage (unless it is in an unnamed namespace).

An unnamed class defined in this way should only contain C-compatible
constructs. In particular, it must not
- declare any members other than non-static data members, member enumerations,
  or member classes,
- have any base classes or default member initializers, or
- contain a lambda expression,
and all member classes must also satisfy these requirements (recursively).
*(since C++20)*

### Keywords

`typedef`

### Notes

Type aliases provide the same functionality as typedef declarations using a
different syntax, and are also applicable to template names.
*(since C++11)*

### Example

```cpp
// simple typedef
typedef unsigned long ulong;

// the following two objects have the same type
unsigned long l1;
ulong l2;

// more complicated typedef
typedef int int_t, *intp_t, (&fp)(int, ulong), arr_t[10];

// the following two objects have the same type
int a1[10];
arr_t a2;

// beware: the following two objects do not have the same type
const intp_t p1 = 0; // int *const p1 = 0
const int *p2;

// common C idiom to avoid having to write "struct S"
typedef struct { int a; int b; } S, *pS;

// the following two objects have the same type
pS ps1;
S* ps2;

// error: storage-class-specifier cannot appear in a typedef declaration
// typedef static unsigned int uint;

// typedef can be used anywhere in the decl-specifier-seq
long unsigned typedef int long ullong;
// more conventionally spelled "typedef unsigned long long int ullong;"

// std::add_const, like many other metafunctions, use member typedefs
template<class T>
struct add_const
{
    typedef const T type;
};

typedef struct Node
{
    struct listNode* next; // declares a new (incomplete) struct type named listNode
} listNode; // error: conflicts with the previously declared struct name

// C++20 error: "struct with typedef name for linkage" has member functions
typedef struct { void f() {} } C_Incompatible;
```

### Defect Reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 576 | C++98 | typedef was not allowed in the entire function definition |
      allowed in function body
  CWG 2071 | C++98 | typedef could appear in a declaration that does not contain
      a declarator | now disallowed

### See also

- Type alias
- Alias template

**C documentation for `Typedef declaration`**

---
*Source: https://en.cppreference.com/w/cpp/language/typedef*
