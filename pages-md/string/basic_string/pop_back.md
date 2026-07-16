# std::basic_string<CharT,Traits,Allocator>::pop_back

```cpp
void pop_back();  // (until C++20)
constexpr void pop_back();  // (since C++20)
```

Removes the last character from the string.

Equivalent to `erase(end() - 1)`. The behavior is undefined if the string is
empty.

### Parameters

(none)

### Return value

(none)

### Complexity

Constant.

### Exceptions

Throws nothing.

### Notes

In libstdc++, `pop_back()` is not available in C++98 mode.

### Example

```cpp
#include <cassert>
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::string str("Short string!");
    std::cout << "before=" << std::quoted(str) << '\n';
    assert(str.size() == 13);

    str.pop_back();
    std::cout << " after=" << std::quoted(str) << '\n';
    assert(str.size() == 12);

    str.clear();
//  str.pop_back(); // undefined behavior
}
```

Output:

```text
before="Short string!"
 after="Short string"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 534 | C++98 | `std::basic_string` did not have the member function
      `pop_back()` | added

### See also

- **push_back** — appends a character to the end (public member function)
- **erase** — removes characters (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/pop_back*
