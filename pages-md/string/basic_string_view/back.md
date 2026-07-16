# std::basic_string_view<CharT,Traits>::back

```cpp
constexpr const_reference back() const;  // (since C++17)
```

Returns reference to the last character in the view. The behavior is undefined
if `empty() == true`.

### Parameters

(none)

### Return value

Reference to the last character, equivalent to `operator[](size() - 1)`.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    for (std::string_view str{"ABCDEF"}; !str.empty(); str.remove_suffix(1))
        std::cout << str.back() << ' ' << str << '\n';
}
```

Output:

```text
F ABCDEF
E ABCDE
D ABCD
C ABC
B AB
A A
```

### See also

- **front** — accesses the first character (public member function)
- **empty** — checks whether the view is empty (public member function)
- **back (DR*)** — accesses the last character (public member function of
  `std::basic_string<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/back*
