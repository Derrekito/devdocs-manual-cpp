# std::basic_string<CharT,Traits,Allocator>::back

```cpp
CharT& back();  // (until C++20)
constexpr CharT& back();  // (since C++20)
const CharT& back() const;  // (until C++20)
constexpr const CharT& back() const;  // (since C++20)
```

Returns reference to the last character in the string. The behavior is undefined
if `empty()` is true.

### Parameters

(none)

### Return value

Reference to the last character, equivalent to `operator[](size() - 1)`.

### Complexity

Constant.

### Notes

In libstdc++, `back()` is not available in C++98 mode.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s("Exemplary");
    char& back1 = s.back();
    back1 = 's';
    std::cout << s << '\n'; // "Exemplars"

    std::string const c("Exemplary");
    char const& back2 = c.back();
    std::cout << back2 << '\n'; // 'y'
}
```

Output:

```text
Exemplars
y
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 534 | C++98 | `std::basic_string` did not have the member function
      `back()` | added

### See also

- **front (DR*)** — accesses the first character (public member function)
- **back** — accesses the last character (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/back*
