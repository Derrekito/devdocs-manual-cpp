# std::is_class

```cpp
template< class T >
struct is_class;  // (since C++11)
```

`std::is_class` is a UnaryTypeTrait.

Checks whether `T` is a non-union class type. Provides the member constant
`value` which is equal to `true`, if `T` is a class type (but not union).
Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for `std::is_class` or
`std::is_class_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_class_v = is_class<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a non-union class type, `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Possible implementation

```cpp
namespace detail
{
    template<class T>
    std::integral_constant<bool, !std::is_union<T>::value> test(int T::*);

    template<class>
    std::false_type test(...);
}

template<class T>
struct is_class : decltype(detail::test<T>(nullptr)) {};
```

### Example

```cpp
#include <iostream>
#include <type_traits>

struct A {};

class B {};

enum class E {};

union U { class UC {}; };
static_assert(not std::is_class_v<U>);
static_assert(std::is_class_v<U::UC>);

int main()
{
    std::cout << std::boolalpha;
    std::cout << std::is_class<A>::value << ": A\n";
    std::cout << std::is_class_v<B> << ": B\n";
    std::cout << std::is_class_v<B*> << ": B*\n";
    std::cout << std::is_class_v<B&> << ": B&\n";
    std::cout << std::is_class_v<const B> << ": const B\n";
    std::cout << std::is_class<E>::value << ": E\n";
    std::cout << std::is_class_v<int> << ": int\n";
    std::cout << std::is_class_v<struct S> << ": struct S (incomplete)\n";
    std::cout << std::is_class_v<class C> << ": class C (incomplete)\n";
}
```

Output:

```text
true: A
true: B
false: B*
false: B&
true: const B
false: E
false: int
true: struct S (incomplete)
true: class C (incomplete)
```

### See also

- **is_union (C++11)** — checks if a type is a union type (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_class*
