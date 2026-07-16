# std::source_location::line

```cpp
constexpr std::uint_least32_t line() const noexcept;  // (since C++20)
```

Returns the line number represented by this object.

### Parameters

(none)

### Return value

The line number represented by this object.

An implementation is encouraged to return `0` when the line number is unknown.

### Example

```cpp
#include <iostream>
#include <string_view>
#include <source_location>

inline void cur_line(
    const std::string_view message = "",
    const std::source_location& location = std::source_location::current())
{
    std::cout
        << location.line() // <- the line # of a call site
        << ") "
        << message;
}

int main()
{
    cur_line("++\n");
    cur_line(); std::cout << "Hello,\n";
    cur_line(); std::cout << "C++20!\n";
    cur_line("--\n");
}
```

Output:

```text
17) ++
18) Hello,
19) C++20!
20) --
```

### See also

- **column** — return the column number represented by this object (public
  member function)
- **file_name** — return the file name represented by this object (public member
  function)
- **function_name** — return the name of the function represented by this
  object, if any (public member function)
- **source_line (C++23)** — gets the line number that lexically relates the
  evaluation represented by the `stacktrace_entry` (public member function of
  `std::stacktrace_entry`)

**Filename and line information**

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location/line*
