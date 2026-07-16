# std::basic_string_view<CharT,Traits>::front

```cpp
constexpr const_reference front() const;  // (since C++17)
```

Returns reference to the first character in the view. The behavior is undefined
if `empty() == true`.

### Parameters

(none)

### Return value

Reference to the first character, equivalent to `operator[](0)`.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    for (std::string_view str{"ABCDEF"}; !str.empty(); str.remove_prefix(1))
        std::cout << str.front() << ' ' << str << '\n';
}
```

Output:

```text
A ABCDEF
B BCDEF
C CDEF
D DEF
E EF
F F
```

### See also

- **back** — accesses the last character (public member function)
- **empty** — checks whether the view is empty (public member function)
- **front (DR*)** — accesses the first character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/front*
