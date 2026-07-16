# std::basic_ios<CharT,Traits>::imbue

```cpp
std::locale imbue( const std::locale& loc );
```

Replaces the current locale. Effectively calls `ios_base::imbue(loc)` and if
there is an associated stream buffer (`rdbuf() != 0`), then calls
`rdbuf()->pubimbue(loc)`.

### Parameters

- **loc** — the new locale

### Return value

The previous locale, as returned by `ios_base::imbue(loc)`.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <iostream>
#include <locale>
#include <sstream>

int main()
{
    std::istringstream iss;
    iss.imbue(std::locale("en_US.UTF8"));

    std::cout << "Current locale: " << iss.getloc().name() << '\n';

    iss.imbue(std::locale());
    std::cout << "Global locale : " << iss.getloc().name() << '\n';
}
```

Output:

```text
Current locale: en_US.UTF8
Global locale : C
```

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/imbue*
