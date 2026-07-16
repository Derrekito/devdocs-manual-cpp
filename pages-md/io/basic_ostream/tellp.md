# std::basic_ostream<CharT,Traits>::tellp

```cpp
pos_type tellp();
```

Returns the output position indicator of the current associated `streambuf`
object.

Behaves as UnformattedOutputFunction (except without actually performing
output). After constructing and checking the sentry object,
*(since C++11)*

If `fail()==true`, returns `pos_type(-1)`. Otherwise, returns
`rdbuf()->pubseekoff(0, std::ios_base::cur, std::ios_base::out)`.

### Parameters

(none)

### Return value

current output position indicator on success, `pos_type(-1)` if a failure
occurs.

### Example

```cpp
#include <iostream>
#include <sstream>
int main()
{
    std::ostringstream s;
    std::cout << s.tellp() << '\n';
    s << 'h';
    std::cout << s.tellp() << '\n';
    s << "ello, world ";
    std::cout << s.tellp() << '\n';
    s << 3.14 << '\n';
    std::cout << s.tellp() << '\n' << s.str();
}
```

Output:

```text
0
1
13
18
hello, world 3.14
```

### See also

- **seekp** — sets the output position indicator (public member function)
- **tellg** — returns the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)
- **seekg** — sets the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/tellp*
