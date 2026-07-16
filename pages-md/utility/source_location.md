# std::source_location

```cpp
struct source_location;  // (since C++20)
```

The `std::source_location` class represents certain information about the source
code, such as file names, line numbers, and function names. Previously,
functions that desire to obtain this information about the call site (for
logging, testing, or debugging purposes) must use macros so that predefined
macros like `__LINE__` and `__FILE__` are expanded in the context of the caller.
The `std::source_location` class provides a better alternative.

`std::source_location` meets the DefaultConstructible, CopyConstructible,
CopyAssignable and Destructible requirements. Lvalue of `std::source_location`
meets the Swappable requirement.

Additionally, the following conditions are `true`:

- `std::is_nothrow_move_constructible_v<std::source_location>`,
- `std::is_nothrow_move_assignable_v<std::source_location>`, and
- `std::is_nothrow_swappable_v<std::source_location>`.

It is intended that `std::source_location` has a small size and can be copied
efficiently.

It is unspecified whether the copy/move constructors and the copy/move
assignment operators of `std::source_location` are trivial and/or constexpr.

### Member functions

**Creation**

- **(constructor)** — constructs a new `source_location` with
  implementation-defined values (public member function)
- **current [static]** — constructs a new `source_location` corresponding to the
  location of the call site (public static member function)

**Field access**

- **line** — return the line number represented by this object (public member
  function)
- **column** — return the column number represented by this object (public
  member function)
- **file_name** — return the file name represented by this object (public member
  function)
- **function_name** — return the name of the function represented by this
  object, if any (public member function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_source_location` | 201907L | (C++20) | Source-code information
      capture (`std::source_location`)

### Example

```cpp
#include <iostream>
#include <source_location>
#include <string_view>

void log(const std::string_view message,
         const std::source_location location =
               std::source_location::current())
{
    std::clog << "file: "
              << location.file_name() << '('
              << location.line() << ':'
              << location.column() << ") `"
              << location.function_name() << "`: "
              << message << '\n';
}

template<typename T>
void fun(T x)
{
    log(x);
}

int main(int, char*[])
{
    log("Hello world!");
    fun("Hello C++20!");
}
```

Possible output:

```text
file: main.cpp(23:8) `int main(int, char**)`: Hello world!
file: main.cpp(18:8) `void fun(T) [with T = const char*]`: Hello C++20!
```

### See also

- **#line** — changes the source code's line number and, optionally, the current
  file name (preprocessing directive)
- **stacktrace_entry (C++23)** — representation of an evaluation in a stacktrace
  (class)

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location*
