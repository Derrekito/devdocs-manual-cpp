# NULL

```cpp
#define NULL /* implementation-defined */
```

The macro `NULL` is an implementation-defined null pointer constant.

### Possible implementation

```cpp
#define NULL 0
//since C++11
#define NULL nullptr
```

### Notes

In C, the macro `NULL` may have the type void*, but that is not allowed in C++
because null pointer constants cannot have that type.

Some implementations define `NULL` as the compiler extension `__null` with
following properties:

- `__null` is equivalent to a zero-valued integer literal (and thus compatible
  with the C++ standard) and has the same size as void*, e.g. it is equivalent
  to `​0​`/`0L` on ILP32/LP64 platforms respectively;
- conversion from `__null` to an arithmetic type, including the type of `__null`
  itself, may trigger a warning.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <type_traits>
#include <typeinfo>

class S;

int main()
{
    int* p = NULL;
    int* p2 = static_cast<std::nullptr_t>(NULL);
    void(*f)(int) = NULL;
    int S::*mp = NULL;
    void(S::*mfp)(int) = NULL;
    auto nullvar = NULL; // may trigger a warning when compiling with gcc/clang

    std::cout << "The type of nullvar is " << typeid(nullvar).name() << '\n';

    if constexpr(std::is_same_v<decltype(NULL), std::nullptr_t>)
        std::cout << "NULL implemented with type std::nullptr_t\n";
    else
        std::cout << "NULL implemented using an integral type\n";

    [](...){}(p, p2, f, mp, mfp); // < suppresses "unused variable" warnings
}
```

Possible output:

```text
The type of nullvar is long
NULL implemented using an integral type
```

### See also

- **`nullptr`(C++11)** — the pointer literal which specifies a null pointer
  value
- **nullptr_t (C++11)** — the type of the null pointer literal `nullptr`
  (typedef)

**C documentation for `NULL`**

---
*Source: https://en.cppreference.com/w/cpp/types/NULL*
