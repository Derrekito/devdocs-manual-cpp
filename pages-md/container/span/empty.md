# std::span<T,Extent>::empty

```cpp
[[nodiscard]] constexpr bool empty() const noexcept;  // (since C++20)
```

Checks if the span is empty.

### Parameters

(none)

### Return value

`true` if the span is empty (i.e., `size() == 0`); `false` otherwise.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <span>

int main()
{
    std::span<const char> span{"ABCDEF"};

    while (!span.empty())
    {
        std::cout << std::quoted(span.data()) << '\n';
        span = span.last(span.size() - 1);
    }
}
```

Output:

```text
"ABCDEF"
"BCDEF"
"CDEF"
"DEF"
"EF"
"F"
""
```

### See also

- **size (C++20)** — returns the number of elements in the sequence (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/empty*
