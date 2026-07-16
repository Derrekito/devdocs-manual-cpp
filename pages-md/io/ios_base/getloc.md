# std::ios_base::getloc

```cpp
std::locale getloc() const;
```

Returns the current locale associated with the stream.

### Parameters

(none)

### Return value

The locale object associated with the stream.

### Example

The output shown was obtained using the clang compiler.

```cpp
#include <codecvt>
#include <ctime>
#include <iomanip>
#include <iostream>

int main()
{
    std::wbuffer_convert<std::codecvt_utf8<wchar_t>> conv(std::cout.rdbuf());
    std::wostream out(&conv);

    out.imbue(std::locale(out.getloc(),
                          new std::time_put_byname<wchar_t>("ja_JP.utf8")));

    std::time_t t = std::time(nullptr);
    out << std::put_time(std::localtime(&t), L"%A %c") << '\n';
}
```

Possible output:

```text
木曜日 2023年10月05日 19時47分58秒
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 47 | C++98 | the return value was misspecified as the return value of
      `imbue()` | corrected

### See also

- **imbue** — sets locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/getloc*
