# Type

Objects, references, functions including function template specializations, and
expressions have a property called *type*, which both restricts the operations
that are permitted for those entities and provides semantic meaning to the
otherwise generic sequences of bits.

### Type classification

The C++ type system consists of the following types:

- fundamental types (see also `std::is_fundamental`):

- the type `std::nullptr_t` (see also `std::is_null_pointer`);
*(since C++11)*

- arithmetic types (see also `std::is_arithmetic`):
- integral types / integer types (including cv-qualified versions, see also
  `std::is_integral`):
- the type bool;
- character types:
- narrow character types:

- the type char8_t
*(since C++20)*

- wide character types: `char16_t`, `char32_t`, (since C++11)wchar_t;
- signed integer types:

- extended signed integer types (implementation-defined);
*(since C++11)*

- unsigned integer types:

- extended unsigned integer types (each corresponds to an extended signed
  integer type, and vice versa);
*(since C++11)*

- floating-point types (see also `std::is_floating_point`):

- extended floating-point types (including cv-qualified versions):
*(since C++23)*

- compound types (see also `std::is_compound`):
- reference types (see also `std::is_reference`):
- lvalue reference types (see also `std::is_lvalue_reference`):

- rvalue reference types (see also `std::is_rvalue_reference`):
*(since C++11)*

- pointer types (see also `std::is_pointer`):
- pointer-to-member types (see also `std::is_member_pointer`):
- array types (see also `std::is_array`);
- function types (see also `std::is_function`);
- enumeration types (see also `std::is_enum`);

- scoped enumeration types (see also `std::is_scoped_enum`);
*(since C++11)*

- class types:

1. `signed char` and `unsigned char` are narrow character types, but they are
   not character types. In other words, the set of narrow character types is not
   a subset of the set of character types.

For every non-cv-qualified type other than reference and function, the type
system supports three additional cv-qualified versions of that type (const,
volatile, and const volatile).

Types are grouped in various categories based on their properties:

- object types are (possibly cv-qualified) types that are not function types,
  reference types, or possibly cv-qualified void (see also `std::is_object`);
- scalar types are (possibly cv-qualified) object types that are not array types
  or class types (see also `std::is_scalar`);
- trivial types (see also `std::is_trivial`), POD types (see also
  `std::is_pod`), literal types (see also `std::is_literal_type`), and other
  categories listed in the type traits library or as named type requirements.

Constructing a complete object type such that the number of bytes in its object
representation is not representable in the type `std::size_t` (i.e. the result
type of `sizeof` operator) is ill-formed.

### Type naming

A name can be declared to refer to a type by means of:

- class declaration;
- union declaration;
- enum declaration;
- typedef declaration;
- type alias declaration.

Types that do not have names often need to be referred to in C++ programs; the
syntax for that is known as type-id. The syntax of the type-id that names type
`T` is exactly the syntax of a declaration of a variable or function of type
`T`, with the identifier omitted, except that decl-specifier-seq of the
declaration grammar is constrained to type-specifier-seq, and that new types may
be defined only if the type-id appears on the right-hand side of a non-template
type alias declaration.

```cpp
int* p;               // declaration of a pointer to int
static_cast<int*>(p); // type-id is "int*"

int a[3];   // declaration of an array of 3 int
new int[3]; // type-id is "int[3]" (called new-type-id)

int (*(*x[2])())[3];      // declaration of an array of 2 pointers to functions
                          // returning pointer to array of 3 int
new (int (*(*[2])())[3]); // type-id is "int (*(*[2])())[3]"

void f(int);                    // declaration of a function taking int and returning void
std::function<void(int)> x = f; // type template parameter is a type-id "void(int)"
std::function<auto(int) -> void> y = f; // same

std::vector<int> v;       // declaration of a vector of int
sizeof(std::vector<int>); // type-id is "std::vector<int>"

struct { int x; } b;         // creates a new type and declares an object b of that type
sizeof(struct { int x; });   // error: cannot define new types in a sizeof expression
using t = struct { int x; }; // creates a new type and declares t as an alias of that type

sizeof(static int); // error: storage class specifiers not part of type-specifier-seq
std::function<inline void(int)> f; // error: neither are function specifiers
```

The declarator part of the declaration grammar with the name removed is referred
to as abstract-declarator.

Type-id may be used in the following situations:

- to specify the target type in cast expressions;
- as arguments to sizeof, alignof, alignas, new, and typeid;
- on the right-hand side of a type alias declaration;
- as the trailing return type of a function declaration;
- as the default argument of a template type parameter;
- as the template argument for a template type parameter;

- in dynamic exception specification.
*(until C++17)*

Type-id can be used with some modifications in the following situations:

- in the parameter list of a function (when the parameter name is omitted),
  type-id uses decl-specifier-seq instead of type-specifier-seq (in particular,
  some storage class specifiers are allowed);
- in the name of a user-defined conversion function, the abstract declarator
  cannot include function or array operators.

### Elaborated type specifier

Elaborated type specifiers may be used to refer to a previously-declared class
name (class, struct, or union) or to a previously-declared enum name even if the
name was hidden by a non-type declaration. They may also be used to declare new
class names.

See elaborated type specifier for details.

### Static type

The type of an expression that results from the compile-time analysis of the
program is known as the *static type* of the expression. The static type does
not change while the program is executing.

### Dynamic type

If some glvalue expression refers to a polymorphic object, the type of its most
derived object is known as the dynamic type.

```cpp
// given
struct B { virtual ~B() {} }; // polymorphic type
struct D : B {};               // polymorphic type

D d; // most-derived object
B* ptr = &d;

// the static type of (*ptr) is B
// the dynamic type of (*ptr) is D
```

For prvalue expressions, the dynamic type is always the same as the static type.

### Incomplete type

The following types are *incomplete types*:

- the type void (possibly cv-qualified);
- *incompletely-defined object types*: class type that has been declared (e.g.
  by forward declaration) but not defined; array of unknown bound; array of
  elements of incomplete type; enumeration type from the point of declaration
  until its underlying type is determined.

All other types are complete.

Any of the following contexts requires type `T` to be complete:

- definition of or call to a function with return type `T` or argument type `T`;
- definition of an object of type `T`;
- declaration of a non-static class data member of type `T`;
- new-expression for an object of type `T` or an array whose element type is
  `T`;
- lvalue-to-rvalue conversion applied to a glvalue of type `T`;
- an implicit or explicit conversion to type `T`;
- a standard conversion, dynamic_cast, or static_cast to type `T*` or `T&`,
  except when converting from the null pointer constant or from a pointer to
  possibly cv-qualified void;
- class member access operator applied to an expression of type `T`;
- typeid, sizeof, or alignof operator applied to type `T`;
- arithmetic operator applied to a pointer to `T`;
- definition of a class with base class `T`;
- assignment to an lvalue of type `T`;
- a catch-clause for an exception of type `T`, `T&`, or `T*`.

(In general, when the size and layout of `T` must be known.)

If any of these situations occur in a translation unit, the definition of the
type must appear in the same translation unit. Otherwise, it is not required.

An incompletely-defined object type can be completed:

- A class type (such as class X) might be regarded as incomplete at one point in
  a translation unit and regarded as complete later on; the type class X is the
  same type at both points:

```cpp
struct X;            // declaration of X, no definition provided yet
extern X* xp;        // xp is a pointer to an incomplete type:
                     // the definition of X is not reachable

void foo()
{
    xp++;            // ill-formed: X is incomplete
}

struct X { int i; }; // definition of X
X x;                 // OK: the definition of X is reachable

void bar()
{
    xp = &x;         // OK: type is “pointer to X”
    xp++;            // OK: X is complete
}
```

- The declared type of an array object might be an array of incomplete class
  type and therefore incomplete; if the class type is completed later on in the
  translation unit, the array type becomes complete; the array type at those two
  points is the same type.
- The declared type of an array object might be an array of unknown bound and
  therefore be incomplete at one point in a translation unit and complete later
  on; the array types at those two points ("array of unknown bound of `T`" and
  "array of `N T`") are different types.

The type of a pointer or reference to array of unknown bound permanently points
to or refers to an incomplete type. An array of unknown bound named by a
`typedef` declaration permanently refers to an incomplete type. In either case,
the array type cannot be completed:

```cpp
extern int arr[];   // the type of arr is incomplete
typedef int UNKA[]; // UNKA is an incomplete type

UNKA* arrp;         // arrp is a pointer to an incomplete type
UNKA** arrpp;

void foo()
{
    arrp++;         // error: UNKA is an incomplete type
    arrpp++;        // OK: sizeof UNKA* is known
}

int arr[10];        // now the type of arr is complete

void bar()
{
    arrp = &arr;    // OK: qualification conversion (since C++20)
    arrp++;         // error: UNKA cannot be completed
}
```

### Defect Reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 328 | C++98 | class members of incomplete type were not prohibited if an
      object of the class type was never created | non-static class data members
      need to be complete
  CWG 977 | C++98 | the point when an enumeration type becomes complete in its
      definition was unclear | the type is complete once the underlying type is
      determined
  CWG 1362 | C++98 | user-defined conversions to type `T*` or `T&` required `T`
      to be complete | not required
  CWG 1464 | C++98 | object size might be not representable in `std::size_t` |
      such type is ill-formed
  CWG 2006 | C++98 | cv-qualified void types were object type and complete type
      | excluded from both categories
  CWG 2448 | C++98 | only cv-unqualified types could be integral and
      floating-point types | allowed cv-qualified types
  CWG 2630 | C++98 | it was unclear whether a class is considered complete
      outside the translation unit where the definition of the class appears |
      the class is complete if its definition is reachable in this case
  CWG 2643 | C++98 | the type of a pointer to array of unknown bound could not
      be completed (but it is already complete) | the pointed-to array type
      cannot be completed

### See also

**C documentation for Type**

---
*Source: https://en.cppreference.com/w/cpp/language/type*
