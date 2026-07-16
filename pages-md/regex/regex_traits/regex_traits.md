# std::regex_traits<CharT>::regex_traits

```cpp
regex_traits();
```

Default-constructs the `std::regex_traits` object, including
default-constructing the private `std::locale` member or any other internal
state as necessary.

### Parameters

(none)

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <regex>

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::regex_traits<char> r1;
    std::regex_traits<wchar_t> r2;
    std::cout << "The regex locale is " << r1.getloc().name() << '\n';
}
```

Output:

```text
The regex locale is en_US.utf8
```

---
*Source: https://en.cppreference.com/w/cpp/regex/regex_traits/regex_traits*
