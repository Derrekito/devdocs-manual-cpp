# std::is_rvalue_reference

```cpp
template< class T >
struct is_rvalue_reference;  // (since C++11)
```

`std::is_rvalue_reference` is a UnaryTypeTrait.

Checks whether `T` is an rvalue reference type. Provides the member constant
`value` which is equal to `true`, if `T` is an rvalue reference type. Otherwise,
`value` is equal to `false`.

The behavior of a program that adds specializations for
`std::is_rvalue_reference` or `std::is_rvalue_reference_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_rvalue_reference_v = is_rvalue_reference<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an rvalue reference type, `false`
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
template<class T> struct is_rvalue_reference : std::false_type {};
template<class T> struct is_rvalue_reference<T&&> : std::true_type {};
```

### Example

```cpp
#include <type_traits>
#include <iostream>

class A {};

static_assert
(
    std::is_rvalue_reference_v<A> == false and
    std::is_rvalue_reference_v<A&> == false and
    std::is_rvalue_reference_v<A&&> != false and
    std::is_rvalue_reference_v<char> == false and
    std::is_rvalue_reference_v<char&> == false and
    std::is_rvalue_reference_v<char&&> != false
);

template <typename T>
void test(T&& x)
{
    static_assert(std::is_same_v<T&&, decltype(x)>);
    std::cout << "T\t" << std::is_rvalue_reference<T>::value << '\n';
    std::cout << "T&&\t" << std::is_rvalue_reference<T&&>::value << '\n';
    std::cout << "decltype(x)\t" << std::is_rvalue_reference<decltype(x)>::value << '\n';
}

int main()
{
    std::cout << std::boolalpha;
    std::cout << "A\t" << std::is_rvalue_reference<A>::value << '\n';
    std::cout << "A&\t" << std::is_rvalue_reference<A&>::value << '\n';
    std::cout << "A&&\t" << std::is_rvalue_reference<A&&>::value << '\n';
    std::cout << "char\t" << std::is_rvalue_reference<char>::value << '\n';
    std::cout << "char&\t" << std::is_rvalue_reference<char&>::value << '\n';
    std::cout << "char&&\t" << std::is_rvalue_reference<char&&>::value << '\n';

    std::cout << "\ntest(42)\n";
    test(42);

    std::cout << "\ntest(x)\n";
    int x = 42;
    test(x);
}
```

Output:

```text
A        false
A&        false
A&&        true
char        false
char&        false
char&&        true

test(42)
T        false
T&&        true
decltype(x)        true

test(x)
T        false
T&&        false
decltype(x)        false
```

### See also

- **is_lvalue_reference (C++11)** — checks if a type is an *lvalue reference*
  (class template)
- **is_reference (C++11)** — checks if a type is either an *lvalue reference* or
  *rvalue reference* (class template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_rvalue_reference*
