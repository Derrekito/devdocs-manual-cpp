# C++ attribute: no_unique_address (since C++20)

Allows this data member to be overlapped with other non-static data members or
base class subobjects of its class.

### Syntax

```text
[[no_unique_address]]
```

### Explanation

Applies to the name being declared in the declaration of a non-static data
member that's not a bit-field.

Makes this member subobject potentially-overlapping, i.e., allows this member to
be overlapped with other non-static data members or base class subobjects of its
class. This means that if the member has an empty class type (e.g. stateless
allocator), the compiler may optimise it to occupy no space, just like if it
were an empty base. If the member is not empty, any tail padding in it may be
also reused to store other data members.

### Notes

`[[no_unique_address]]` is ignored by MSVC even in C++20 mode; instead,
`[[msvc::no_unique_address]]` is provided.

### Example

```cpp
#include <iostream>

struct Empty {}; // empty class

struct X
{
    int i;
    Empty e;
};

struct Y
{
    int i;
    [[no_unique_address]] Empty e;
};

struct Z
{
    char c;
    [[no_unique_address]] Empty e1, e2;
};

struct W
{
    char c[2];
    [[no_unique_address]] Empty e1, e2;
};

int main()
{
    // the size of any object of empty class type is at least 1
    static_assert(sizeof(Empty) >= 1);

    // at least one more byte is needed to give e a unique address
    static_assert(sizeof(X) >= sizeof(int) + 1);

    // empty member optimized out
    std::cout << "sizeof(Y) == sizeof(int) is " << std::boolalpha
              << (sizeof(Y) == sizeof(int)) << '\n';

    // e1 and e2 cannot share the same address because they have the
    // same type, even though they are marked with [[no_unique_address]].
    // However, either may share address with c.
    static_assert(sizeof(Z) >= 2);

    // e1 and e2 cannot have the same address, but one of them can share with
    // c[0] and the other with c[1]
    std::cout << "sizeof(W) == 2 is " << (sizeof(W) == 2) << '\n';
}
```

Possible output:

```text
sizeof(Y) == sizeof(int) is true
sizeof(W) == 2 is true
```

---
*Source: https://en.cppreference.com/w/cpp/language/attributes/no_unique_address*
