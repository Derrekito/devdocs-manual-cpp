# std::has_unique_object_representations

```cpp
template< class T >
struct has_unique_object_representations;  // (since C++17)
```

`std::has_unique_object_representations` is a UnaryTypeTrait.

If `T` is TriviallyCopyable and if any two objects of type `T` with the same
value have the same object representation, provides the member constant `value`
equal `true`. For any other type, `value` is `false`.

For the purpose of this trait, two arrays have the same value if their elements
have the same values, two non-union classes have the same value if their direct
subobjects have the same value, and two unions have the same value if they have
the same active member and the value of that member is the same.

It is implementation-defined which scalar types satisfy this trait, but
unsigned(until C++20) integer types that do not use padding bits are guaranteed
to have unique object representations.

The behavior is undefined if `T` is an incomplete type other than (possibly
cv-qualified) `void` or array of unknown bound.

The behavior of a program that adds specializations for
`std::has_unique_object_representations` or
`std::has_unique_object_representations_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool has_unique_object_representations_v =
    has_unique_object_representations<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` has unique object representations, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

This trait was introduced to make it possible to determine whether a type can be
correctly hashed by hashing its object representation as a byte array.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_has_unique_object_representations` | 201606L | (C++17) |
      `std::has_unique_object_representations`

### Example

```cpp
#include <cstdint>
#include <type_traits>

struct unpadded
{
    std::uint32_t a, b;
};

struct likely_padded
{
    std::uint8_t c;
    std::uint16_t st;
    std::uint32_t i;
};

int main()
{
    // Every value of a char corresponds to exactly one object representation.
    static_assert(std::has_unique_object_representations_v<char>);
    // For IEC 559 floats, assertion passes because the value NaN has
    // multiple object representations.
    static_assert(!std::has_unique_object_representations_v<float>);

    // Should succeed in any sane implementation because unpadded
    // is typically not padded, and std::uint32_t cannot contain padding bits.
    static_assert(std::has_unique_object_representations_v<unpadded>);
    // Fails in most implementations because padding bits are inserted
    // between the data members c and st for the purpose of aligning st to 16 bits.
    static_assert(!std::has_unique_object_representations_v<likely_padded>);

    // Notable architectural divergence:
    static_assert(std::has_unique_object_representations_v<bool>);  // x86
 // static_assert(!std::has_unique_object_representations_v<bool>); // ARM
}
```

### See also

- **is_standard_layout (C++11)** — checks if a type is a standard-layout type
  (class template)
- **hash (C++11)** — hash function object (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/has_unique_object_representations*
