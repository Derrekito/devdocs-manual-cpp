# Object

C++ programs create, destroy, refer to, access, and manipulate *objects*.

An object, in C++, has

- size (can be determined with `sizeof`);
- alignment requirement (can be determined with `alignof`);
- storage duration (automatic, static, dynamic, thread-local);
- lifetime (bounded by storage duration or temporary);
- type;
- value (which may be indeterminate, e.g. for default-initialized non-class
  types);
- optionally, a name.

The following entities are not objects: value, reference, function, enumerator,
type, non-static class member, template, class or function template
specialization, namespace, parameter pack, and `this`.

A *variable* is an object or a reference that is not a non-static data member,
that is introduced by a declaration.

### Object creation

Objects can be explicitly created by definitions, new-expressions,
throw-expressions, changing the active member of a union and evaluating
expressions that require temporary objects. The created object is uniquely
defined in explicit object creation.

Objects of implicit-lifetime types can also be implicitly created by

- operations that begin lifetime of an array of type unsigned char or
  `std::byte`(since C++17), in which case such objects are created in the array,
- call to following allocating functions, in which case such objects are created
  in the allocated storage:

- `std::aligned_alloc`
*(since C++17)*

- call to following object representation copying functions, in which case such
  objects are created in the destination region of storage or the result:

- `std::bit_cast`
*(since C++20)*

Zero or more objects may be created in the same region of storage, as long as
doing so would give the program defined behavior. If such creation is
impossible, e.g. due to conflicting operations, the behavior of the program is
undefined. If multiple such sets of implicitly created objects would give the
program defined behavior, it is unspecified which such set of objects is
created. In other words, implicitly created objects are not required to be
uniquely defined.

After implicitly creating objects within a specified region of storage, some
operations produce a pointer to a *suitable created object*. The suitable
created object has the same address as the region of storage. Likewise, the
behavior is undefined if only if no such pointer value can give the program
defined behavior, and it is unspecified which pointer value is produced if there
are multiple values giving the program defined behavior.

```cpp
#include <cstdlib>

struct X { int a, b; };

X* MakeX()
{
    // One of possible defined behaviors:
    // the call to std::malloc implicitly creates an object of type X
    // and its subobjects a and b, and returns a pointer to that X object
    X* p = static_cast<X*>(std::malloc(sizeof(X)));
    p->a = 1;
    p->b = 2;
    return p;
}
```

Call to `std::allocator::allocate` or implicitly defined copy/move special
member functions of union types can also create objects. The functions
`std::start_lifetime_as` and `std::start_lifetime_as_array` also implicitly
create objects of a specified type and an array of a specified type respectively
at the location of the pointer passed to them and return a pointer to the object
and to the first element of the array respectively.(since C++23)

### Object representation and value representation

Some types and objects have *object representations* and *value
representations*, they are defined in the table below:

  Entity | Object representation | Value representation
  a complete object type `T` | the sequence of `N` unsigned char objects taken
      up by a non-bit-field complete object of type `T`, where `N` is
      `sizeof(T)` | the set of bits in the object representation of `T` that
      participate in representing a value of type `T`
  a non-bit-field complete object `obj` of type `T` | the bytes of `obj`
      corresponding to the object representation of `T` | the bits of `obj`
      corresponding to the value representation of `T`
  a bit-field object `bf` | the sequence of `N` bits taken up by `bf`, where `N`
      is the width of the bit-field | the set of bits in the object
      representation of `bf` that participate in representing the value of `bf`

Bits in the object representation of a type or object that are not part of the
value representation are *padding bits*.

For TriviallyCopyable types, value representation is a part of the object
representation, which means that copying the bytes occupied by the object in the
storage is sufficient to produce another object with the same value (except if
the object is a potentially-overlapping subobject, or the value is a *trap
representation* of its type and loading it into the CPU raises a hardware
exception, such as SNaN ("signalling not-a-number") floating-point values or NaT
("not-a-thing") integers).

Although most implementations do not allow trap representations, padding bits,
or multiple representations for integer types, there are exceptions; for example
a value of an integer type on Itanium may be a trap representation.

The reverse is not necessarily true: two objects of a TriviallyCopyable type
with different object representations may represent the same value. For example,
multiple floating-point bit patterns represent the same special value NaN. More
commonly, padding bits may be introduced to satisfy alignment requirements,
bit-field sizes, etc.

```cpp
#include <cassert>

struct S
{
    char c;  // 1 byte value
             // 3 bytes of padding bits (assuming alignof(float) == 4)
    float f; // 4 bytes value (assuming sizeof(float) == 4)

    bool operator==(const S& arg) const // value-based equality
    {
        return c == arg.c && f == arg.f;
    }
};

void f()
{
    assert(sizeof(S) == 8);
    S s1 = {'a', 3.14};
    S s2 = s1;
    reinterpret_cast<unsigned char*>(&s1)[2] = 'b'; // modify some padding bits
    assert(s1 == s2); // value did not change
}
```

For the objects of type char, signed char, and unsigned char (unless they are
oversize bit-fields), every bit of the object representation is required to
participate in the value representation and each possible bit pattern represents
a distinct value (no padding bits, trap bits, or multiple representations
allowed).

### Subobjects

An object can have *subobjects*. These include

- member objects
- base class subobjects
- array elements

An object that is not a subobject of another object is called *complete object*.

A subobject is *potentially overlapping* if it is a base class subobject or a
non-static data member declared with the `[[no_unique_address]]` attribute(since
C++20).

Complete objects, member objects, and array elements are also known as *most
derived objects*, to distinguish them from base class subobjects. The size of an
object that is neither potentially overlapping nor a bit-field is required to be
non-zero (the size of a base class subobject may be zero even without
`[[no_unique_address]]`(since C++20): see empty base optimization).

An object can contain other objects, in which case the contained objects are
*nested within* the former object. An object `a` is nested within another object
`b` if

- `a` is a subobject of `b`, or
- `b` provides storage for `a`, or
- there exists an object `c` where `a` is nested within `c`, and `c` is nested
  within `b`.

Any two objects with overlapping lifetimes (that are not bit-fields) are
guaranteed to have different addresses unless one of them is nested within
another, or if they are subobjects of different type within the same complete
object, and one of them is a subobject of zero size.

```cpp
static const char c1 = 'x';
static const char c2 = 'x';
assert(&c1 != &c2); // same values, different addresses
```

For a class,

- its non-static data members,
- its non-virtual direct base classes, and,
- if the class is not abstract, its virtual base classes

are called its *potentially constructed subobjects*.

### Polymorphic objects

Objects of a class type that declares or inherits at least one virtual function
are polymorphic objects. Within each polymorphic object, the implementation
stores additional information (in every existing implementation, it is one
pointer unless optimized out), which is used by virtual function calls and by
the RTTI features (`dynamic_cast` and `typeid`) to determine, at run time, the
type with which the object was created, regardless of the expression it is used
in.

For non-polymorphic objects, the interpretation of the value is determined from
the expression in which the object is used, and is decided at compile time.

```cpp
#include <iostream>
#include <typeinfo>

struct Base1
{
    // polymorphic type: declares a virtual member
    virtual ~Base1() {}
};

struct Derived1 : Base1
{
     // polymorphic type: inherits a virtual member
};

struct Base2
{
     // non-polymorphic type
};

struct Derived2 : Base2
{
     // non-polymorphic type
};

int main()
{
    Derived1 obj1; // object1 created with type Derived1
    Derived2 obj2; // object2 created with type Derived2

    Base1& b1 = obj1; // b1 refers to the object obj1
    Base2& b2 = obj2; // b2 refers to the object obj2

    std::cout << "Expression type of b1: " << typeid(decltype(b1)).name() << '\n'
              << "Expression type of b2: " << typeid(decltype(b2)).name() << '\n'
              << "Object type of b1: " << typeid(b1).name() << '\n'
              << "Object type of b2: " << typeid(b2).name() << '\n'
              << "Size of b1: " << sizeof b1 << '\n'
              << "Size of b2: " << sizeof b2 << '\n';
}
```

Possible output:

```text
Expression type of b1: Base1
Expression type of b2: Base2
Object type of b1: Derived1
Object type of b2: Base2
Size of b1: 8
Size of b2: 1
```

### Strict aliasing

Accessing an object using an expression of a type other than the type with which
it was created is undefined behavior in many cases, see `reinterpret_cast` for
the list of exceptions and examples.

### Alignment

Every object type has the property called *alignment requirement*, which is a
nonnegative integer value (of type `std::size_t`, and always a power of two)
representing the number of bytes between successive addresses at which objects
of this type can be allocated.

The alignment requirement of a type can be queried with `alignof` or
`std::alignment_of`. The pointer alignment function `std::align` can be used to
obtain a suitably-aligned pointer within some buffer, and `std::aligned_storage`
can be used to obtain suitably-aligned storage.
*(since C++11)*

Each object type imposes its alignment requirement on every object of that type;
stricter alignment (with larger alignment requirement) can be requested using
`alignas`(since C++11). Attempting to create an object in storage that does not
meet the alignment requirements of the object's type is undefined behavior.

In order to satisfy alignment requirements of all non-static members of a class,
padding bits may be inserted after some of its members.

```cpp
#include <iostream>

// objects of type S can be allocated at any address
// because both S.a and S.b can be allocated at any address
struct S
{
    char a; // size: 1, alignment: 1
    char b; // size: 1, alignment: 1
}; // size: 2, alignment: 1

// objects of type X must be allocated at 4-byte boundaries
// because X.n must be allocated at 4-byte boundaries
// because int's alignment requirement is (usually) 4
struct X
{
    int n;  // size: 4, alignment: 4
    char c; // size: 1, alignment: 1
    // three bytes of padding bits
}; // size: 8, alignment: 4

int main()
{
    std::cout << "alignof(S) = " << alignof(S) << '\n'
              << "sizeof(S)  = " << sizeof(S) << '\n'
              << "alignof(X) = " << alignof(X) << '\n'
              << "sizeof(X)  = " << sizeof(X) << '\n';
}
```

Possible output:

```text
alignof(S) = 1
sizeof(S)  = 2
alignof(X) = 4
sizeof(X)  = 8
```

The weakest alignment (the smallest alignment requirement) is the alignment of
char, signed char, and unsigned char, which equals `1`; the largest *fundamental
alignment* of any type is implementation-defined and equal to the alignment of
`std::max_align_t`(since C++11).

Fundamental alignments are supported for objects of all kinds of storage
durations.

If a type's alignment is made stricter (larger) than `std::max_align_t` using
`alignas`, it is known as a type with *extended alignment* requirement. A type
whose alignment is extended or a class type whose non-static data member has
extended alignment is an *over-aligned type*.
Allocator types are required to handle over-aligned types correctly.
*(since C++11)*

It is implementation-defined if new-expressions and(until C++17)
`std::get_temporary_buffer` support over-aligned types.
*(since C++11)(until C++20)*

### Notes

Objects in C++ have different meaning from objects in object-oriented
programming (OOP):

  Objects in C++ | Objects in OOP
  can have any object type (see `std::is_object`) | must have a class type
  no concept of "instance" | have the concept of "instance" (and there are
      mechanisms like `instanceof` to detect "instance-of" relationship)
  no concept of "interface" | have the concept of "interface" (and there are
      mechanisms like `instanceof` to detect whether an interface is
      implemented)
  polymorphism needs to be explicitly enabled via virtual members | polymorphism
      is always enabled

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 633 | C++98 | variables could only be objects | they can also be
      references
  CWG 734 | C++98 | it was unspecified whether variables defined in the same
      scope that are guaranteed to have the same value can have the same address
      | address is guaranteed to be different if their lifetimes overlap,
      regardless of their values
  CWG 1189 | C++98 | two base class subobjects of the same type could have the
      same address | they always have distinct addresses
  CWG 1861 | C++98 | for oversize bit-fields of narrow character types, all bits
      of the object representation still participated in the value
      representation | allows padding bits
  CWG 2489 | C++98 | char[] cannot provide storage, but objects could be
      implicitly created within its storage | objects cannot be implicitly
      created within the storage of char[]
  CWG 2519 | C++98 | the definition of object representation did not address
      bit-fields | addresses bit-fields
  CWG 2719 | C++98 | the behavior of creating an object in misaligned storage
      was unclear | the behavior is undefined in this case
  P0593R6 | C++98 | previous object model did not support many useful idioms
      required by the standard library and was not compatible with effective
      types in C | implicit object creation added

### See also

**C documentation for Object**

---
*Source: https://en.cppreference.com/w/cpp/language/object*
