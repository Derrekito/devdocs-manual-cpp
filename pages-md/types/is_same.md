# std::is_same

```cpp
template< class T, class U >
struct is_same;  // (since C++11)
```

If `T` and `U` name the same type (taking into account const/volatile
qualifications), provides the member constant `value` equal to `true`. Otherwise
`value` is `false`.

Commutativity is satisfied, i.e. for any two types `T` and `U`, `is_same<T,
U>::value == true` if and only if `is_same<U, T>::value == true`.

The behavior of a program that adds specializations for `std::is_same` or
`std::is_same_v`(since C++17) is undefined.

### Helper variable template

```cpp
template< class T, class U >
inline constexpr bool is_same_v = is_same<T, U>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` and `U` are the same type, `false`
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
template<class T, class U>
struct is_same : std::false_type {};

template<class T>
struct is_same<T, T> : std::true_type {};
```

### Example

```cpp
#include <cstdint>
#include <iostream>
#include <type_traits>

int main()
{
    std::cout << std::boolalpha;

    // some implementation-defined facts

    // usually true if 'int' is 32 bit
    std::cout << std::is_same<int, std::int32_t>::value << ' '; // maybe true
    // possibly true if ILP64 data model is used
    std::cout << std::is_same<int, std::int64_t>::value << ' '; // maybe false

    // same tests as above, except using C++17's std::is_same_v<T, U> format
    std::cout << std::is_same_v<int, std::int32_t> << ' ';  // maybe true
    std::cout << std::is_same_v<int, std::int64_t> << '\n'; // maybe false

    // compare the types of a couple variables
    long double num1 = 1.0;
    long double num2 = 2.0;
    static_assert( std::is_same_v<decltype(num1), decltype(num2)> == true );

    // 'float' is never an integral type
    static_assert( std::is_same<float, std::int32_t>::value == false );

    // 'int' is implicitly 'signed'
    static_assert( std::is_same_v<int, int> == true );
    static_assert( std::is_same_v<int, unsigned int> == false );
    static_assert( std::is_same_v<int, signed int> == true );

    // unlike other types, 'char' is neither 'unsigned' nor 'signed'
    static_assert( std::is_same_v<char, char> == true );
    static_assert( std::is_same_v<char, unsigned char> == false );
    static_assert( std::is_same_v<char, signed char> == false );

    // const-qualified type T is not same as non-const T
    static_assert( !std::is_same<const int, int>() );
}
```

Possible output:

```text
true false true false
```

### See also

- **same_as (C++20)** — specifies that a type is the same as another type
  (concept)
- **`decltype` specifier(C++11)** — obtains the type of an expression or an
  entity

---
*Source: https://en.cppreference.com/w/cpp/types/is_same*
