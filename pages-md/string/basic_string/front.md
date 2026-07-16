# std::basic_string<CharT,Traits,Allocator>::front

```cpp
CharT& front();  // (until C++20)
constexpr CharT& front();  // (since C++20)
const CharT& front() const;  // (until C++20)
constexpr const CharT& front() const;  // (since C++20)
```

Returns reference to the first character in the string. The behavior is
undefined if `empty()` is true.

### Parameters

(none)

### Return value

Reference to the first character, equivalent to `operator[](0)`.

### Complexity

Constant.

### Notes

In libstdc++, `front()` is not available in C++98 mode.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s("Exemplary");
    char& f1 = s.front();
    f1 = 'e';
    std::cout << s << '\n'; // "exemplary"

    std::string const c("Exemplary");
    char const& f2 = c.front();
    std::cout << &f2 << '\n'; // "Exemplary"
}
```

Output:

```text
exemplary
Exemplary
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 534 | C++98 | `std::basic_string` did not have the member function
      `front()` | added

### See also

- **back (DR*)** — accesses the last character (public member function)
- **front** — accesses the first character (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/front*
