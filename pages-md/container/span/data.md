# std::span<T,Extent>::data

```cpp
constexpr pointer data() const noexcept;  // (since C++20)
```

Returns a pointer to the beginning of the sequence.

### Return value

A pointer to the beginning of the sequence.

### Example

```cpp
#include <iostream>
#include <span>

int main()
{
    constexpr char str[] = "ABCDEF\n";

    const std::span sp{str};

    for (auto n{sp.size()}; n != 2; --n)
        std::cout << sp.last(n).data();
}
```

Output:

```text
ABCDEF
BCDEF
CDEF
DEF
EF
F
```

### See also

- **(constructor)** — constructs a `span` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/data*
