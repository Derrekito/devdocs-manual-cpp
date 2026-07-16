# offsetof

```cpp
#define offsetof(type, member) /* implementation-defined */
```

The macro `offsetof` expands to an integral constant expression of type
`std::size_t`, the value of which is the offset, in bytes, from the beginning of
an object of specified type to its specified subobject, including padding bits
if any.

Given an object `o` of type `type` and static storage duration, `o.member` shall
be an lvalue constant expression that refers to a subobject of `o`. Otherwise,
the behavior is undefined. Particularly, if `member` is a static data member, a
bit-field, or a member function, the behavior is undefined.

If `type` is not a PODType(until C++11)standard-layout type(since C++11), the
result of `offsetof` is undefined(until C++17)use of the `offsetof` macro is
conditionally-supported(since C++17).

The expression `offsetof(type, member)` is never type-dependent and it is
value-dependent if and only if `type` is dependent.

### Exceptions

`offsetof` throws no exceptions.

The expression `noexcept(offsetof(type, member))` always evaluates to `true`.
*(since C++11)*

### Notes

The offset of the first member of a standard-layout type is always zero
(empty-base optimization is mandatory).
*(since C++11)*

`offsetof` cannot be implemented in standard C++ and requires compiler support:
GCC, LLVM.

`member` is not restricted to a direct member. It can denote a subobject of a
given member, such as an element of an array member. This is specified by C DR
496.

It is specified in C23 that defining a new type containg an unparenthesized
comma in `offsetof` is undefined behavior, and such usage is generally not
supported by implementations in C++ modes: `offsetof(struct Foo { int a, b; },
a)` is rejected by all known implementations.

### Example

```cpp
#include <cstddef>
#include <iostream>

struct S
{
    char   m0;
    double m1;
    short  m2;
    char   m3;
//  private: int z; // warning: 'S' is a non-standard-layout type
};

int main()
{
    std::cout
        << "offset of char   m0 = " << offsetof(S, m0) << '\n'
        << "offset of double m1 = " << offsetof(S, m1) << '\n'
        << "offset of short  m2 = " << offsetof(S, m2) << '\n'
        << "offset of char   m3 = " << offsetof(S, m3) << '\n';
}
```

Possible output:

```text
offset of char   m0 = 0
offset of double m1 = 8
offset of short  m2 = 16
offset of char   m3 = 18
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 273 | C++98 | `offsetof` may not work if unary `operator&` is overloaded |
      required to work correctly even if `operator&` is overloaded
  LWG 306 | C++98 | the behavior was not specified when `type` is not a PODType
      | the result is undefined in this case
  LWG 449 | C++98 | other requirements of `offsetof` were removed by the
      resolution of LWG issue 306 | added them back

### See also

- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)
- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)

**C documentation for `offsetof`**

---
*Source: https://en.cppreference.com/w/cpp/types/offsetof*
