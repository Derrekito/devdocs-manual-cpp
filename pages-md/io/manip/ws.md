# std::ws

```cpp
template< class CharT, class Traits >
std::basic_istream<CharT, Traits>& ws( std::basic_istream<CharT, Traits>& is );
```

Discards leading whitespace from an input stream.

Behaves as an UnformattedInputFunction, except that `is.gcount()` is not
modified. After constructing and checking the sentry object, extracts characters
from the stream and discards them until any one of the following conditions
occurs:

- end of file condition occurs in the input sequence (in which case the function
  calls `setstate(eofbit)` but does not set `failbit`; this does not apply if
  the `eofbit` is already set on `is` prior to the call to `ws`, in which case
  the construction of the sentry object would set `failbit`).
- the next available character `c` in the input sequence is not whitespace as
  determined by `std::isspace(c, is.getloc())`. The non-whitespace character is
  not extracted.

This is an input-only I/O manipulator, it may be called with an expression such
as `in >> std::ws` for any `in` of type `std::basic_istream`.

### Parameters

- **is** — reference to input stream

### Return value

`is` (reference to the stream after extraction of consecutive whitespace).

### Notes

If `eofbit` is set on the stream prior to the call, the construction of the
sentry object will set `failbit`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <istream>
#include <sstream>
#include <string>

int main()
{
    for (const char* str : {"     #1 test", "\t #2 test", "#3 test"})
    {
        std::string line;
        std::getline(std::istringstream{str}, line);
        std::cout << "getline returns:\t" << std::quoted(line) << '\n';

        std::istringstream iss{str};
        std::getline(iss >> std::ws, line);
        std::cout << "ws + getline returns:\t" << std::quoted(line) << '\n';
    }
}
```

Output:

```text
getline returns:        "     #1 test"
ws + getline returns:        "#1 test"
getline returns:        "         #2 test"
ws + getline returns:        "#2 test"
getline returns:        "#3 test"
ws + getline returns:        "#3 test"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 415 | C++98 | calling `std::ws` might not construct the sentry object
      (insonsistent with other input functions) | required to construct the
      sentry object

### See also

- **ignore** — extracts and discards characters until the given character is
  found (public member function of `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/ws*
