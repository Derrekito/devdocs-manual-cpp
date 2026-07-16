# alignof operator (since C++11)

Queries alignment requirements of a type.

### Syntax

```text
alignof( type-id )
```

Returns a value of type `std::size_t`.

### Explanation

Returns the alignment, in bytes, required for any instance of the type indicated
by type-id, which is either complete object type, an array type whose element
type is complete, or a reference type to one of those types.

If the type is reference type, the operator returns the alignment of referenced
type; if the type is array type, alignment requirement of the element type is
returned.

### Notes

See alignment for the meaning and properties of the value returned by `alignof`.

### Keywords

`alignof`

### Example

```cpp
#include <iostream>

struct Foo
{
    int   i;
    float f;
    char  c;
};

// Note: `alignas(alignof(long double))` below can be simplified to simply
// `alignas(long double)` if desired.
struct alignas(alignof(long double)) Foo2
{
    // put your definition here
};

struct Empty {};

struct alignas(64) Empty64 {};

int main()
{
    std::cout << "Alignment of"  "\n"
        "- char            : " << alignof(char)    << "\n"
        "- pointer         : " << alignof(int*)    << "\n"
        "- class Foo       : " << alignof(Foo)     << "\n"
        "- class Foo2      : " << alignof(Foo2)    << "\n"
        "- empty class     : " << alignof(Empty)   << "\n"
        "- empty class\n"
        "  with alignas(64): " << alignof(Empty64) << "\n";
}
```

Possible output:

```text
Alignment of
- char            : 1
- pointer         : 8
- class Foo       : 4
- class Foo2      : 16
- empty class     : 1
- empty class
  with alignas(64): 64
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  CWG 1305 | C++11 | type-id could not represent a reference to an array with an
      unknown bound but a complete element type | allowed

### See also

- **alignment requirement** — restricts the addresses at which an object may be
  allocated
- **`alignas` specifier(C++11)** — specifies that the storage for the variable
  should be aligned by specific amount
- **alignment_of (C++11)** — obtains the type's alignment requirements (class
  template)

**C documentation for `_Alignof`**

---
*Source: https://en.cppreference.com/w/cpp/language/alignof*
