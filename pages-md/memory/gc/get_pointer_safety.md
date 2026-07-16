# std::get_pointer_safety

```cpp
std::pointer_safety get_pointer_safety() noexcept;  // (since C++11) (removed in C++23)
```

Obtains the implementation-defined pointer safety model, which is a value of
type `std::pointer_safety`.

### Parameters

(none)

### Return value

The pointer safety used by this implementation.

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::cout << "Pointer safety: ";
    switch (std::get_pointer_safety())
    {
        case std::pointer_safety::strict:
            std::cout << "strict\n";
            break;
        case std::pointer_safety::preferred:
            std::cout << "preferred\n";
            break;
        case std::pointer_safety::relaxed:
            std::cout << "relaxed\n";
            break;
    }
}
```

Possible output:

```text
Pointer safety: relaxed
```

### See also

- **pointer_safety (C++11)(removed in C++23)** — lists pointer safety models
  (enum)

---
*Source: https://en.cppreference.com/w/cpp/memory/gc/get_pointer_safety*
