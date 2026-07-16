# Standard library header <iostream>

`<iostream>` is part of the I/O library. Include it whenever you want
to read from `std::cin` or write to `std::cout`, `std::cerr`, or
`std::clog` — it's the header that actually declares those global
stream objects (the stream *types* themselves live in `<istream>` /
`<ostream>` / `<ios>`, which `<iostream>` pulls in for you).

Including `<iostream>` behaves as if it defines a static-duration
`std::ios_base::Init` object, whose constructor initializes the
standard stream objects the first time it runs, and whose destructor
flushes them (except `cin`/`wcin`) when the last such object is
destroyed. This construction is guaranteed to happen — a defect report
(LWG 1123) fixed earlier wording that left it unclear, and another
(LWG 155) corrected the object's stated type from
`std::basic_ios::Init` to `std::ios_base::Init`.

### Includes

`<iostream>` pulls in and re-exports:

- **`<ios>`** (C++11 as a standalone header) — `std::ios_base`,
  `std::basic_ios`, and related typedefs
- **`<streambuf>`** (C++11) — `std::basic_streambuf`
- **`<istream>`** (C++11) — `std::basic_istream` and its typedefs
- **`<ostream>`** (C++11) — `std::basic_ostream`, `std::basic_iostream`,
  and their typedefs

### Objects

- **`cin` / `wcin`** — reads from the standard C stream `stdin`
- **`cout` / `wcout`** — writes to the standard C stream `stdout`
- **`cerr` / `wcerr`** — writes to the standard C stream `stderr`,
  unbuffered
- **`clog` / `wclog`** — writes to the standard C stream `stderr`,
  buffered

### Reference

```cpp skip
#include <ios>
#include <streambuf>
#include <istream>
#include <ostream>

namespace std {
  extern istream cin;
  extern ostream cout;
  extern ostream cerr;
  extern ostream clog;

  extern wistream wcin;
  extern wostream wcout;
  extern wostream wcerr;
  extern wostream wclog;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/iostream*
