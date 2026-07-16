# Standard library header <sstream>

`<sstream>` provides in-memory streams that read from or write to a
`std::string` instead of a file or the console — the standard way to
build strings with `<<` formatting, or to parse a string field by
field with `>>`. Construct one from an existing string to parse it,
or default-construct and call `str()` afterward to retrieve what was
written.

### Stream classes

- **`basic_istringstream`** (**`istringstream`**, **`wistringstream`**)
  — parse a string via `>>`
- **`basic_ostringstream`** (**`ostringstream`**, **`wostringstream`**)
  — build a string via `<<`
- **`basic_stringstream`** (**`stringstream`**, **`wstringstream`**) —
  both directions on the same buffer

### Buffer

- **`basic_stringbuf`** (**`stringbuf`**, **`wstringbuf`**) — the raw
  device backing the stream classes above; holds the actual
  `std::string`, exposed via `str()` / `view()`

### Functions

- **`std::swap`** (C++11) — overloads for each class above, swapping
  two streams' (or buffers') contents

---
*Source: https://en.cppreference.com/w/cpp/header/sstream*
