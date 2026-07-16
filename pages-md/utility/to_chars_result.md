# std::to_chars_result

```cpp
struct to_chars_result;  // (since C++17)
```

`std::to_chars_result` is the return type of `std::to_chars`. It has no base
classes, and only has the following members.

### Data members

- **`ptr`** — const char*
- **`ec`** — `std::errc`

### Member and friend functions

## operator==(std::to_chars_result)

```cpp
friend bool operator==( const to_chars_result&,
                        const to_chars_result& ) = default;  // (since C++20)
```

Compares the two arguments using default comparisons (which uses operator== to
compare `ptr` and `ec` respectively).

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::to_chars_result` is an
associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

## operator bool

```cpp
constexpr explicit operator bool() const noexcept;  // (since C++26)
```

Checks whether the conversion is successful. Returns `ec == std::errc{}`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_to_chars` | 201611L | (C++17) | Elementary string conversions
      (`std::to_chars`, `std::from_chars`)
  202306L | (C++26) | Testing for success or failure of `<charconv>` functions

### Example

```cpp
#include <array>
#include <charconv>
#include <iostream>
#include <string_view>
#include <system_error>

void show_to_chars(auto... format_args)
{
    std::array<char, 10> str;

#if __cpp_lib_to_chars >= 202306L
    // use C++26 operator bool() for error checking
    if (auto res = std::to_chars(str.data(), str.data() + str.size(), format_args...))
        std::cout << std::string_view(str.data(), res.ptr) << '\n';
    else
        std::cout << std::make_error_code(res.ec).message() << '\n';
#else
    if (auto [ptr, ec]
            = std::to_chars(str.data(), str.data() + str.size(), format_args...);
        ec == std::errc())
        std::cout << std::string_view(str.data(), ptr) << '\n';
    else
        std::cout << std::make_error_code(ec).message() << '\n';
#endif
}

int main()
{
    show_to_chars(42);
    show_to_chars(+3.14159F);
    show_to_chars(-3.14159, std::chars_format::fixed);
    show_to_chars(-3.14159, std::chars_format::scientific, 3);
    show_to_chars(3.1415926535, std::chars_format::fixed, 10);
}
```

Possible output:

```text
42
3.14159
-3.14159
-3.142e+00
Value too large for defined data type
```

### See also

- **to_chars (C++17)** — converts an integer or floating-point value to a
  character sequence (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/to_chars_result*
