# std::source_location::file_name

```cpp
constexpr const char* file_name() const noexcept;  // (since C++20)
```

Returns the name of the current source file represented by this object,
represented as a null-terminated byte string.

### Parameters

(none)

### Return value

The name of the current source file represented by this object, represented as a
null-terminated byte string.

### Example

```cpp
#include <iostream>
#include <source_location>

void print_this_file_name(
    std::source_location location = std::source_location::current())
{
    // Name of file that contains the call site of this function.
    std::cout << "File: " << location.file_name() << '\n';
}

int main()
{
#line 1 "cppreference.cpp"
    print_this_file_name();
}
```

Output:

```text
File: cppreference.cpp
```

### See also

- **line** — return the line number represented by this object (public member
  function)
- **column** — return the column number represented by this object (public
  member function)
- **function_name** — return the name of the function represented by this
  object, if any (public member function)
- **source_file (C++23)** — gets the name of the source file that lexically
  contains the expression or statement whose evaluation is represented by the
  `stacktrace_entry` (public member function of `std::stacktrace_entry`)

**Filename and line information**

---
*Source: https://en.cppreference.com/w/cpp/utility/source_location/file_name*
