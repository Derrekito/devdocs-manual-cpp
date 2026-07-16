# Standard library header <iostream>

This header is part of the Input/output library.

Including `<iostream>` behaves as if it defines a static storage duration object
of type `std::ios_base::Init`, whose constructor initializes the standard stream
objects if it is the first `std::ios_base::Init` object to be constructed, and
whose destructor flushes those objects (except for `cin` and `wcin`) if it is
the last `std::ios_base::Init` object to be destroyed.

**Includes**

- **<ios> (C++11)** — `std::ios_base` class, `std::basic_ios` class template and
  several typedefs
- **<streambuf> (C++11)** — `std::basic_streambuf` class template
- **<istream> (C++11)** — `std::basic_istream` class template and several
  typedefs
- **<ostream> (C++11)** — `std::basic_ostream`, `std::basic_iostream` class
  templates and several typedefs

**Objects**

- **cinwcin** — reads from the standard C input stream `stdin` (global object)
- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **cerrwcerr** — writes to the standard C error stream `stderr`, unbuffered
  (global object)
- **clogwclog** — writes to the standard C error stream `stderr` (global object)

### Synopsis

```cpp
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

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 155 | C++98 | the type of the static object was `std::basic_ios::Init`
      (not strictly wrong, but the wording was misleading) | corrected to
      `std::ios_base::Init`
  LWG 1123 | C++98 | including `<iostream>` did not guarantee the construction
      of `std::ios_base::Init` objects | guarantees

---
*Source: https://en.cppreference.com/w/cpp/header/iostream*
