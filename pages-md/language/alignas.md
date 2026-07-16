# `alignas` specifier (since C++11)

Specifies the alignment requirement of a type or an object.

### Syntax

```text
alignas( expression )
alignas( type-id )
alignas( pack ... )
```

1) expression must be an integral constant expression that evaluates to zero, or
   to a valid value for an alignment or extended alignment.

2) Equivalent to `alignas(alignof(type-id))`.

3) Equivalent to multiple alignas specifiers applied to the same declaration,
   one for each member of the parameter pack, which can be either type or
   non-type parameter pack.

### Explanation

The `alignas` specifier may be applied to:

- the declaration or definition of a class;
- the declaration of a non-bitfield class data member;
- the declaration of a variable, except that it cannot be applied to the
  following: a function parameter; the exception parameter of a catch clause.

The object or the type declared by such a declaration will have its alignment
requirement equal to the strictest (largest) non-zero expression of all
`alignas` specifiers used in the declaration, unless it would weaken the natural
alignment of the type.

If the strictest (largest) `alignas` on a declaration is weaker than the
alignment it would have without any `alignas` specifiers (that is, weaker than
its natural alignment or weaker than `alignas` on another declaration of the
same object or type), the program is ill-formed:

```cpp
struct alignas(8) S {};
struct alignas(1) U { S s; }; // error: alignment of U would have been 8 without alignas(1)
```

Invalid non-zero alignments, such as `alignas(3)` are ill-formed.

Valid non-zero alignments that are weaker than another `alignas` on the same
declaration are ignored.

`alignas(0)` is always ignored.

### Notes

As of the ISO C11 standard, the C language has the `_Alignas` keyword and
defines `alignas` as a preprocessor macro expanding to the keyword in the header
`<stdalign.h>`.

In C++, this is a keyword, and

the headers `<stdalign.h>` and `<cstdalign>` do not define such macro. They do,
however, define the macro constant `__alignas_is_defined`.
*(until C++20)*

the header `<stdalign.h>` does not define such macro. It does, however, define
the macro constant `__alignas_is_defined`.
*(since C++20)*

### Keywords

`alignas`

### Example

```cpp
#include <iostream>

// every object of type struct_float will be aligned to alignof(float) boundary
// (usually 4):
struct alignas(float) struct_float
{
    // your definition here
};

// every object of type sse_t will be aligned to 32-byte boundary:
struct alignas(32) sse_t
{
    float sse_data[4];
};

// the array "cacheline" will be aligned to 64-byte boundary:
alignas(64) char cacheline[64];

int main()
{
    struct default_aligned
    {
        float data[4];
    } a, b, c;
    sse_t x, y, z;

    std::cout
        << "alignof(struct_float) = " << alignof(struct_float) << '\n'
        << "sizeof(sse_t) = " << sizeof(sse_t) << '\n'
        << "alignof(sse_t) = " << alignof(sse_t) << '\n'
        << "alignof(cacheline) = " << alignof(alignas(64) char[64]) << '\n'
        << std::hex << std::showbase
        << "&a: " << &a << '\n'
        << "&b: " << &b << '\n'
        << "&c: " << &c << '\n'
        << "&x: " << &x << '\n'
        << "&y: " << &y << '\n'
        << "&z: " << &z << '\n';
}
```

Possible output:

```text
alignof(struct_float) = 4
sizeof(sse_t) = 32
alignof(sse_t) = 32
alignof(cacheline) = 64
&a: 0x7fffcec89930
&b: 0x7fffcec89940
&c: 0x7fffcec89950
&x: 0x7fffcec89960
&y: 0x7fffcec89980
&z: 0x7fffcec899a0
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1437 | C++11 | alignas could be used in alias declarations | prohibited
  CWG 2354 | C++11 | alignas could be applied to the declaration of an
      enumeration | prohibited

### See also

- **`alignof` operator(C++11)** — queries alignment requirements of a type
- **alignment_of (C++11)** — obtains the type's alignment requirements (class
  template)

**C documentation for `_Alignas, alignas`**

---
*Source: https://en.cppreference.com/w/cpp/language/alignas*
