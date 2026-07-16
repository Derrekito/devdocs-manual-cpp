# std::basic_string<CharT,Traits,Allocator>::rbegin, std::basic_string<CharT,Traits,Allocator>::crbegin

```cpp
reverse_iterator rbegin();  // (until C++11)
reverse_iterator rbegin() noexcept;  // (since C++11) (until C++20)
constexpr reverse_iterator rbegin() noexcept;  // (since C++20)
const_reverse_iterator rbegin() const;  // (until C++11)
const_reverse_iterator rbegin() const noexcept;  // (since C++11) (until C++20)
constexpr const_reverse_iterator rbegin() const noexcept;  // (since C++20)
const_reverse_iterator crbegin() const;  // (until C++11)
const_reverse_iterator crbegin() const noexcept;  // (since C++11) (until C++20)
constexpr const_reverse_iterator crbegin() const noexcept;  // (since C++20)
```

Returns a reverse iterator to the first character of the reversed string. It
corresponds to the last character of the non-reversed string.

### Parameters

(none)

### Return value

Reverse iterator to the first character.

### Complexity

Constant.

### Notes

In libstdc++, `crbegin()` is not available in C++98 mode.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string s("Exemplar!");
    *s.rbegin() = 'y';
    std::cout << s << '\n'; // "Exemplary"

    std::string c;
    std::copy(s.crbegin(), s.crend(), std::back_inserter(c));
    std::cout << c << '\n'; // "yralpmexE"
}
```

Output:

```text
Exemplary
yralpmexE
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1192 | C++98 | `std::basic_string` did not have the member function
      `crbegin()` | added

### See also

- **rendcrend (DR*)** — returns a reverse iterator to the end (public member
  function)
- **rbegincrbegin** — returns a reverse iterator to the beginning (public member
  function of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/rbegin*
