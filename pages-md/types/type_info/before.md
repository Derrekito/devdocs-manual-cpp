# std::type_info::before

```cpp
bool before( const type_info& rhs ) const;  // (until C++11)
bool before( const type_info& rhs ) const noexcept;  // (since C++11)
```

Returns `true` if the type of this `type_info` precedes the type of `rhs` in the
implementation's collation order. No guarantees are given; in particular, the
collation order can change between the invocations of the same program.

### Parameters

- **rhs** — another type information object to compare to

### Return value

`true` if the type of this `type_info` precedes the type of `rhs` in the
implementation's collation order.

### Example

```cpp
#include <iostream>
#include <typeinfo>

int main()
{
    if (typeid(int).before(typeid(char)))
        std::cout << "int goes before char in this implementation.\n";
    else
        std::cout << "char goes before int in this implementation.\n";
}
```

Possible output:

```text
char goes before int in this implementation.
```

### See also

- **operator==operator!= (removed in C++20)** — checks whether the objects refer
  to the same type (public member function)

---
*Source: https://en.cppreference.com/w/cpp/types/type_info/before*
