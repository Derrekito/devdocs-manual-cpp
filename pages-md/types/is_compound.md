# std::is_compound

```cpp
template< class T >
struct is_compound;  // (since C++11)
```

`std::is_compound` is a UnaryTypeTrait.

If `T` is a compound type (that is, array, function, object pointer, function
pointer, member object pointer, member function pointer, reference, class,
union, or enumeration, including any cv-qualified variants), provides the member
constant `value` equal `true`. For any other type, `value` is `false`.

The behavior of a program that adds specializations for `std::is_compound` or
`std::is_compound_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_compound_v = is_compound<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a compound type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Compound types are the types that are constructed from fundamental types. Any
C++ type is either fundamental or compound.

### Possible implementation

```cpp
template<class T>
struct is_compound : std::integral_constant<bool, !std::is_fundamental<T>::value> {};
```

### Example

```cpp
#include <type_traits>

static_assert(not std::is_compound_v<int>);
static_assert(std::is_compound_v<int*>);
static_assert(std::is_compound_v<int&>);

void f();
static_assert(std::is_compound_v<decltype(f)>);
static_assert(std::is_compound_v<decltype(&f)>);

static_assert(std::is_compound_v<char['a']>);

class C {};
static_assert(std::is_compound_v<C>);

union U {};
static_assert(std::is_compound_v<U>);

enum struct E { e };
static_assert(std::is_compound_v<E>);
static_assert(std::is_compound_v<decltype(E::e)>);

struct S
{
    int i : 8;
    int j;
    void foo();
};
static_assert(not std::is_compound_v<decltype(S::i)>);
static_assert(not std::is_compound_v<decltype(S::j)>);
static_assert(std::is_compound_v<decltype(&S::j)>);
static_assert(std::is_compound_v<decltype(&S::foo)>);

int main() {}
```

### See also

- **is_fundamental (C++11)** — checks if a type is a fundamental type (class
  template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)
- **is_object (C++11)** — checks if a type is an object type (class template)
- **is_array (C++11)** — checks if a type is an array type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_compound*
