# std::formatter<std::stacktrace_entry>

```cpp
template<>
struct formatter<std::stacktrace_entry>;  // (since C++23)
```

The template specialization of `std::formatter` for `std::stacktrace_entry`
allows users to convert a stacktrace entry object to string using formatting
functions such as `std::format`.

### Format specification

The syntax of format specifications is:

```text
fill-and-align ﻿(optional) width ﻿(optional)
```

fill-and-align and width have the same meaning as in standard format
specification.

The formatted output matches the result of `to_string`, adjusted as appropriate
for the format specifiers.

### Example

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **print (C++23)** — prints to `stdout` or a file stream using formatted
  representation of the arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/formatter*
