# std::nothrow

```cpp
struct nothrow_t {};  // (until C++11)
struct nothrow_t { explicit nothrow_t() = default; };  // (since C++11)
extern const std::nothrow_t nothrow;
```

`std::nothrow_t` is an empty class type used to disambiguate the overloads of
throwing and non-throwing allocation functions. `std::nothrow` is a constant of
it.

### Example

```cpp
#include <iostream>
#include <new>

int main()
{
    try
    {
        while (true)
        {
            new int[100000000ul];   // throwing overload
        }
    }
    catch (const std::bad_alloc& e)
    {
        std::cout << e.what() << '\n';
    }

    while (true)
    {
        int* p = new(std::nothrow) int[100000000ul]; // non-throwing overload
        if (p == nullptr)
        {
            std::cout << "Allocation returned nullptr\n";
            break;
        }
    }
}
```

Output:

```text
std::bad_alloc
Allocation returned nullptr
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2510 | C++11 | the default constructor was non-explicit, which could lead
      to ambiguity | made explicit

### See also

- **operator newoperator new[]** — allocation functions (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/new/nothrow_t*
