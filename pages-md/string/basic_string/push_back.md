# std::basic_string<CharT,Traits,Allocator>::push_back

```cpp
void push_back( CharT ch );  // (until C++20)
constexpr void push_back( CharT ch );  // (since C++20)
```

Appends the given character `ch` to the end of the string.

### Parameters

- **ch** — the character to append

### Return value

(none)

### Complexity

Amortized constant.

### Exceptions

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::string str{"Short string"};
    std::cout << "1) " << std::quoted(str) << ", size: " << str.size() << '\n';

    str.push_back('!');
    std::cout << "2) " << std::quoted(str) << ", size: " << str.size() << '\n';
}
```

Output:

```text
1) "Short string", size: 12
2) "Short string!", size: 13
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 7 | C++98 | (1) the description was missing in the C++ standard (2) the
      parameter type was const CharT | (1) description added (2) changed to
      `CharT`
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **pop_back (DR*)** — removes the last character (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/push_back*
