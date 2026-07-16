# std::locale::name

```cpp
std::string name() const;
```

Returns the name of the locale, which is the name by which it is known to the
operating system, such as `"POSIX"` or `"en_US.UTF8"` or `"English_United
States.1252"`. If the locale is not a copy of a system-supplied locale, the
string `"*"` is returned.

### Return value

The name of the locale or `"*"` if unnamed.

### Example

```cpp
#include <iostream>
#include <locale>
#include <string>

int main()
{
    std::locale loc(std::locale(), new std::ctype<char>);
    std::cout << "The default locale is " << std::locale().name() << '\n'
              << "The user's locale is " << std::locale("").name() << '\n'
              << "A nameless locale is " << loc.name() << '\n';
}
```

Possible output:

```text
The default locale is C
The user's locale is en_US.UTF8
A nameless locale is *
```

### See also

- **(constructor)** — constructs a new locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/name*
