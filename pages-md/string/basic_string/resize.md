# std::basic_string<CharT,Traits,Allocator>::resize

```cpp
void resize( size_type count );  // (until C++20)
constexpr void resize( size_type count );  // (since C++20)
void resize( size_type count, CharT ch );  // (until C++20)
constexpr void resize( size_type count, CharT ch );  // (since C++20)
```

Resizes the string to contain `count` characters.

If the current size is less than `count`, additional characters are appended:

1) Initializes appended characters to `CharT()` (`'\0'` if `CharT` is `char`).

2) Initializes appended characters to `ch`.

If the current size is greater than `count`, the string is reduced to its first
`count` elements.

### Parameters

- **count** — new size of the string
- **ch** — character to initialize the new characters with

### Return value

(none)

### Exceptions

`std::length_error` if `count > max_size()`. Any exceptions thrown by
corresponding `Allocator`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <stdexcept>

int main()
{
    const unsigned desired_length{8};
    std::string long_string("Where is the end?");
    std::string short_string("H");

    std::cout << "Basic functionality:\n"
              << "Shorten:\n"
              << "1. Before: " << std::quoted(long_string) << '\n';
    long_string.resize(desired_length);
    std::cout << "2. After:  " << std::quoted(long_string) << '\n';

    std::cout << "Lengthen with a given value 'a':\n"
              << "3. Before: " << std::quoted(short_string) << '\n';
    short_string.resize(desired_length, 'a');
    std::cout << "4. After:  " << std::quoted(short_string) << '\n';

    std::cout << "Lengthen with char() == " << static_cast<int>(char()) << '\n'
              << "5. Before: " << std::quoted(short_string) << '\n';
    short_string.resize(desired_length + 3);
    std::cout << "6. After:  \"";
    for (char c : short_string)
        std::cout << (c == char() ? '@' : c);
    std::cout << "\"\n\n";

    std::cout << "Errors:\n";
    std::string s;

    try
    {
        // size is OK, no length_error
        // (may throw bad_alloc)
        s.resize(s.max_size() - 1, 'x');
    }
    catch (const std::bad_alloc& ex)
    {
        std::cout << "1. Exception: " << ex.what() << '\n';
    }

    try
    {
        // size is OK, no length_error
        // (may throw bad_alloc)
        s.resize(s.max_size(), 'x');
    }
    catch (const std::bad_alloc& ex)
    {
        std::cout << "2. Exception: " << ex.what() << '\n';
    }

    try
    {
        // size is BAD, throw length_error
        s.resize(s.max_size() + 1, 'x');
    }
    catch (const std::length_error& ex)
    {
        std::cout << "3. Length error: " << ex.what() << '\n';
    }
}
```

Possible output:

```text
Basic functionality:
Shorten:
1. Before: "Where is the end?"
2. After:  "Where is"
Lengthen with a given value 'a':
3. Before: "H"
4. After:  "Haaaaaaa"
Lengthen with char() == 0
5. Before: "Haaaaaaa"
6. After:  "Haaaaaaa@@@"

Errors:
1. Exception: std::bad_alloc
2. Exception: std::bad_alloc
3. Length error: basic_string::_M_replace_aux
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **sizelength** — returns the number of characters (public member function)
- **reserve** — reserves storage (public member function)
- **shrink_to_fit (DR*)** — reduces memory usage by freeing unused memory
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/resize*
