# cv (const and volatile) type qualifiers

Appear in any type specifier, including decl-specifier-seq of declaration
grammar, to specify constness or volatility of the object being declared or of
the type being named.

- const - defines that the type is *constant*.
- volatile - defines that the type is *volatile*.

### Explanation

Any (possibly incomplete) type other than function type or reference type is a
type in a group of the following four distinct but related types:

- A *cv-unqualified* version.
- A *const-qualified* version.
- A *volatile-qualified* version.
- A *const-volatile-qualified* version.

These four types in the same group have the same representation and alignment
requirements.

Array types are considered to have the same cv-qualification as their element
types.

#### const and volatile objects

When an object is first created, the cv-qualifiers used (which could be part of
decl-specifier-seq or part of a declarator in a declaration, or part of type-id
in a new-expression) determine the constness or volatility of the object, as
follows:

- A *const object* is

  - an object whose type is const-qualified, or
  - a non-mutable subobject of a const object.

  Such object cannot be modified: attempt to do so directly is a compile-time
  error, and attempt to do so indirectly (e.g., by modifying the const object
  through a reference or pointer to non-const type) results in undefined behavior.

- A *volatile object* is

  - an object whose type is volatile-qualified,
  - a subobject of a volatile object, or
  - a mutable subobject of a const-volatile object.

  Every access (read or write operation, member function call, etc.) made through
  a glvalue expression of volatile-qualified type is treated as a visible
  side-effect for the purposes of optimization (that is, within a single thread of
  execution, volatile accesses cannot be optimized out or reordered with another
  visible side effect that is sequenced-before or sequenced-after the volatile
  access. This makes volatile objects suitable for communication with a signal
  handler, but not with another thread of execution, see `std::memory_order`). Any
  attempt to access a volatile object through a glvalue of non-volatile type (e.g.
  through a reference or pointer to non-volatile type) results in undefined
  behavior.

- A *const volatile object* is

  - an object whose type is const-volatile-qualified,
  - a non-mutable subobject of a const volatile object,
  - a const subobject of a volatile object, or
  - a non-mutable volatile subobject of a const object.

  Behaves as both a const object and as a volatile object.

Each cv-qualifier (const and volatile) can appear at most once in any
cv-qualifier sequence. For example, const const and volatile const volatile are
not valid cv-qualifier sequences.

### `mutable` specifier

- mutable - permits modification of the class member declared mutable even if
  the containing object is declared const (i.e., the class member is mutable).

May appear in the declaration of a non-static class members of non-reference
non-const type:

```cpp
class X
{
    mutable const int* p; // OK
    mutable int* const q; // ill-formed
    mutable int&       r; // ill-formed
};
```

mutable is used to specify that the member does not affect the externally
visible state of the class (as often used for mutexes, memo caches, lazy
evaluation, and access instrumentation).

```cpp
class ThreadsafeCounter
{
    mutable std::mutex m; // The "M&M rule": mutable and mutex go together
    int data = 0;
public:
    int get() const
    {
        std::lock_guard<std::mutex> lk(m);
        return data;
    }

    void inc()
    {
        std::lock_guard<std::mutex> lk(m);
        ++data;
    }
};
```

### Conversions

There is partial ordering of cv-qualifiers by the order of increasing
restrictions. The type can be said *more* or *less* cv-qualified than:

- *unqualified* < const
- *unqualified* < volatile
- *unqualified* < const volatile
- const < const volatile
- volatile < const volatile

References and pointers to cv-qualified types can be implicitly converted to
references and pointers to more cv-qualified types, see qualification
conversions for details.

To convert a reference or a pointer to a cv-qualified type to a reference or
pointer to a less cv-qualified type, `const_cast` must be used.

### Keywords

`const`, `volatile`, `mutable`

### Notes

The const qualifier used on a declaration of a non-local non-volatile
non-template(since C++14)non-inline(since C++17) variable that is not declared
extern gives it internal linkage. This is different from C where const file
scope variables have external linkage.

The C++ language grammar treats mutable as a storage-class-specifier, rather
than a type qualifier, but it does not affect storage class or linkage.

Some uses of volatile are deprecated:
- lvalue of volatile type as operand of built-in increment/decrement operators;
- lvalue of volatile type as left operand of built-in direct assignment, unless
  the direct assignment expression appears in an unevaluated context or is a
  discarded-value expression;
- volatile object type as function parameter type or return type;
- volatile qualifier in structured binding declaration.
*(since C++20)*

### Example

```cpp
#include <cstdlib>

int main()
{
    int n1 = 0;          // non-const object
    const int n2 = 0;    // const object
    int const n3 = 0;    // const object (same as n2)
    volatile int n4 = 0; // volatile object

    const struct
    {
        int n1;
        mutable int n2;
    } x = {0, 0};        // const object with mutable member

    n1 = 1;   // OK: modifiable object
//  n2 = 2;   // error: non-modifiable object
    n4 = 3;   // OK: treated as a side-effect
//  x.n1 = 4; // error: member of a const object is const
    x.n2 = 4; // OK: mutable member of a const object isn't const

    const int& r1 = n1; // reference to const bound to non-const object
//  r1 = 2; // error: attempt to modify through reference to const
    const_cast<int&>(r1) = 2; // OK: modifies non-const object n1

    const int& r2 = n2; // reference to const bound to const object
//  r2 = 2; // error: attempt to modify through reference to const
//  const_cast<int&>(r2) = 2; // undefined behavior: attempt to modify const object n2

    [](...){}(n3, n4, x, r2); // see also: [[maybe_unused]]

    std::system("g++ -O3 -Wa,-adhln ./main.cpp"); // may issue asm on POSIX systems
}
```

Possible output:

```text
# typical machine code produced on an x86_64 platform
# (only the code that contributes to observable side-effects is emitted)
main:
    movl    $0, -4(%rsp) # volatile int n4 = 0;
    movl    $3, -4(%rsp) # n4 = 3;
    xorl    %eax, %eax   # return 0 (implicit)
    ret
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1428 | C++98 | the definition of 'const object' was based on declaration |
      based on object type
  CWG 1528 | C++98 | there was no requirement on the number of occurrences of
      each cv-qualifier in the same cv-qualifier sequence | at most once for
      each cv-qualifier
  CWG 1799 | C++98 | mutable could be applied to data members not declared
      const, but the members' types may still be const-qualified | cannot apply
      mutable to data members of const-qualified types

### See also

**C documentation for `const` qualifier**

**C documentation for `volatile` qualifier**

---
*Source: https://en.cppreference.com/w/cpp/language/cv*
