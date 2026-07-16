# std::formatter<std::thread::id>

```cpp
template< class CharT >
struct formatter<std::thread::id, CharT>;  // (since C++23)
```

The template specialization of `std::formatter` for the `std::thread::id` class
allows users to convert a thread identifier to its textual representation using
formatting functions.

### Format specification

The syntax of format specifications is:

```text
fill-and-align ﻿(optional) width ﻿(optional)
```

fill-and-align and width have the same meaning as in standard format
specification. The default alignment is `>`.

The formatted output matches the output of `operator<<`, adjusted as appropriate
for the format specifiers.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_formatters` | 202302L | (C++23) | Formatting `std::thread::id` and
      `std::stacktrace`

### Example

```cpp
#include <format>
#include <iostream>
#include <thread>

int main()
{
    std::thread::id this_id = std::this_thread::get_id();
    std::thread::id null_id;

    std::cout << std::format("current thread id: {}\n", this_id);
    std::cout << std::format("{:=^10}\n", null_id);
}
```

Possible output:

```text
current thread id: 140046396632256
====0=====
```

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/id/formatter*
