# std::alignment_of

```cpp
template< class T >
struct alignment_of;  // (since C++11)
```

Provides the member constant `value` equal to the alignment requirement of the
type `T`, as if obtained by an `alignof` expression. If `T` is an array type,
returns the alignment requirements of the element type. If `T` is a reference
type, returns the alignment requirements of the type referred to.

If `alignof(T)` is not a valid expression, the behavior is undefined.

The behavior of a program that adds specializations for `std::alignment_of` or
`std::alignment_of_v`(since C++17) is undefined.

### Helper variable template

```cpp
template< class T >
inline constexpr std::size_t alignment_of_v = alignment_of<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `alignof(T)` (public static member constant)

### Member functions

- **operator std::size_t** — converts the object to std::size_t, returns `value`
  (public member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — std::size_t
- **`type`** — std::integral_constant<std::size_t, value>

### Possible implementation

```cpp
template<class T>
struct alignment_of : std::integral_constant<std::size_t, alignof(T)> {};
```

### Notes

This type trait predates the `alignof` keyword, which can be used to obtain the
same value with less verbosity.

### Example

```cpp
#include <cstdint>
#include <iostream>
#include <type_traits>

struct A {};
struct B
{
    std::int8_t p;
    std::int16_t q;
};

int main()
{
    std::cout << std::alignment_of<A>::value << ' ';
    std::cout << std::alignment_of<B>::value << ' ';
    std::cout << std::alignment_of<int>() << ' '; // alt syntax
    std::cout << std::alignment_of_v<double> << '\n'; // c++17 alt syntax
}
```

Possible output:

```text
1 2 4 8
```

### See also

- **`alignof` operator(C++11)** — queries alignment requirements of a type
- **`alignas` specifier(C++11)** — specifies that the storage for the variable
  should be aligned by specific amount
- **aligned_storage (C++11)(deprecated in C++23)** — defines the type suitable
  for use as uninitialized storage for types of given size (class template)
- **aligned_union (C++11)(deprecated in C++23)** — defines the type suitable for
  use as uninitialized storage for all given types (class template)
- **max_align_t (C++11)** — trivial type with alignment requirement as great as
  any other scalar type (typedef)

---
*Source: https://en.cppreference.com/w/cpp/types/alignment_of*
