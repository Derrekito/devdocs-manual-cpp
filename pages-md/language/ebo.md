# Empty base optimization

Allows the size of an empty base subobject to be zero.

### Explanation

The size of any object or member subobject is required to be at least 1 even if
the type is an empty class type (that is, a class or struct that has no
non-static data members), (unless with `[[no_unique_address]]`, see below)(since
C++20) in order to be able to guarantee that the addresses of distinct objects
of the same type are always distinct.

However, base class subobjects are not so constrained, and can be completely
optimized out from the object layout:

```cpp
struct Base {}; // empty class

struct Derived1 : Base
{
    int i;
};

int main()
{
    // the size of any object of empty class type is at least 1
    static_assert(sizeof(Base) >= 1);

    // empty base optimization applies
    static_assert(sizeof(Derived1) == sizeof(int));
}
```

Empty base optimization is prohibited if one of the empty base classes is also
the type or the base of the type of the first non-static data member, since the
two base subobjects of the same type are required to have different addresses
within the object representation of the most derived type.

A typical example of such situation is the naive implementation of
`std::reverse_iterator` (derived from the empty base `std::iterator`), which
holds the underlying iterator (also derived from `std::iterator`) as its first
non-static data member.

```cpp
struct Base {}; // empty class

struct Derived1 : Base
{
    int i;
};

struct Derived2 : Base
{
    Base c; // Base, occupies 1 byte, followed by padding for i
    int i;
};

struct Derived3 : Base
{
    Derived1 c; // derived from Base, occupies sizeof(int) bytes
    int i;
};

int main()
{
    // empty base optimization does not apply,
    // base occupies 1 byte, Base member occupies 1 byte
    // followed by 2 bytes of padding to satisfy int alignment requirements
    static_assert(sizeof(Derived2) == 2*sizeof(int));

    // empty base optimization does not apply,
    // base takes up at least 1 byte plus the padding
    // to satisfy alignment requirement of the first member (whose
    // alignment is the same as int)
    static_assert(sizeof(Derived3) == 3*sizeof(int));
}
```

If multiple inheritance occurs, then the specific optimizations are compiler
specific. In MSVC, the null base class optimization is applied with and only
with the last null base class, the rest of the null base classes are not applied
with the null base optimization and one byte is allocated. In GCC, no matter how
many empty base classes exist, the empty base class applies the empty base class
optimization without allocating any space and the empty base class address is
the same as the first address of the derived class object.

Empty base optimization is *required* for StandardLayoutTypes in order to
maintain the requirement that the pointer to a standard-layout object, converted
using `reinterpret_cast`, points to its initial member, which is why the
requirements for a standard layout type include "has all non-static data members
declared in the same class (either all in the derived or all in some base)" and
"has no base classes of the same type as the first non-static data member".
*(since C++11)*

The empty member subobjects are permitted to be optimized out just like the
empty bases if they use the attribute `[[no_unique_address]]`. Taking the
address of such member results in an address that may equal the address of some
other member of the same object.
```cpp
struct Empty {}; // empty class
struct X
{
    int i;
    [[no_unique_address]] Empty e;
};
int main()
{
    // the size of any object of empty class type is at least 1
    static_assert(sizeof(Empty) >= 1);
    // empty member optimized out:
    static_assert(sizeof(X) == sizeof(int));
}
```
*(since C++20)*

### Notes

Empty base optimization is commonly used by allocator-aware standard library
classes (`std::vector`, `std::function`, `std::shared_ptr`, etc) to avoid
occupying any additional storage for its allocator member if the allocator is
stateless. This is achieved by storing one of the required data members (e.g.,
`begin`, `end`, or `capacity` pointer for the `vector`) in an equivalent of
`boost::compressed_pair` with the allocator.

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++98 standard (ISO/IEC 14882:1998):

### External links

  More C++ Idioms/Empty Base Optimization — A wikibook

---
*Source: https://en.cppreference.com/w/cpp/language/ebo*
