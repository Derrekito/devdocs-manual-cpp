# std::basic_string<CharT,Traits,Allocator>::copy

```cpp
size_type copy( CharT* dest, size_type count, size_type pos = 0 ) const;  // (until C++20)
constexpr size_type copy( CharT* dest, size_type count, size_type pos = 0 ) const;  // (since C++20)
```

Copies a substring `[``pos``,``pos + count``)` to character string pointed to by
`dest`. If the requested substring lasts past the end of the string, or if
`count == npos`, the copied substring is `[``pos``,``size()``)`.

The resulting character string is not null-terminated.

### Parameters

- **dest** — pointer to the destination character string
- **count** — length of the substring
- **pos** — position of the first character to include

### Return value

Number of characters copied.

### Exceptions

`std::out_of_range` if `pos > size()`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Complexity

Linear in `count`.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string foo("WINE");

    // brace-initialization initializes all characters to 0,
    // providing a null-terminator
    char bar[4]{};

    // do not copy the last char, to guarantee null-termination
    foo.copy(bar, sizeof bar - 1);

    std::cout << bar << '\n'; // requires bar to be null-terminated
}
```

Output:

```text
WIN
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **substr** — returns a substring (public member function)
- **copy** — copies characters (public member function of
  `std::basic_string_view<CharT,Traits>`)
- **copycopy_if (C++11)** — copies a range of elements to a new location
  (function template)
- **memcpy** — copies one buffer to another (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/copy*
