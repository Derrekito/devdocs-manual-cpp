# std::to_chars

```cpp
std::to_chars_result
    to_chars( char* first, char* last,
              /* integer-type */ value, int base = 10 );  // (1) (since C++17) (constexpr since C++23)
std::to_chars_result
    to_chars( char*, char*, bool, int = 10 ) = delete;  // (2) (since C++17)
std::to_chars_result
    to_chars( char* first, char* last, float value );
std::to_chars_result
    to_chars( char* first, char* last, double value );
std::to_chars_result
    to_chars( char* first, char* last, long double value );  // (since C++17) (until C++23)
std::to_chars_result
    to_chars( char* first, char* last, /* floating-point-type */ value );  // (since C++23)
std::to_chars_result
    to_chars( char* first, char* last, float value,
              std::chars_format fmt );
std::to_chars_result
    to_chars( char* first, char* last, double value,
              std::chars_format fmt );
std::to_chars_result
    to_chars( char* first, char* last, long double value,
              std::chars_format fmt );  // (since C++17) (until C++23)
std::to_chars_result
    to_chars( char* first, char* last, /* floating-point-type */ value,
              std::chars_format fmt );  // (since C++23)
std::to_chars_result
    to_chars( char* first, char* last, float value,
              std::chars_format fmt, int precision );
std::to_chars_result
    to_chars( char* first, char* last, double value,
              std::chars_format fmt, int precision );
std::to_chars_result
    to_chars( char* first, char* last, long double value,
              std::chars_format fmt, int precision );  // (since C++17) (until C++23)
std::to_chars_result
    to_chars( char* first, char* last, /* floating-point-type */ value,
              std::chars_format fmt, int precision );  // (since C++23)
```

Converts `value` into a character string by successively filling the range
`[``first``,``last``)`, where `[``first``,``last``)` is required to be a valid
range.

1) Integer formatters: `value` is converted to a string of digits in the given
   `base` (with no redundant leading zeroes). Digits in the range `10..35`
   (inclusive) are represented as lowercase characters `a..z`. If value is less
   than zero, the representation starts with a minus sign. The library provides
   overloads for all cv-unqualified(since C++23) signed and unsigned integer
   types and for the type `char` as the type of the parameter `value`.

2) Overload for `bool` is deleted. `std::to_chars` rejects argument of type
   `bool` because the result would be `"0"`/`"1"` but not `"false"`/`"true"` if
   it is permitted.

3) `value` is converted to a string as if by `std::printf` in the default ("C")
   locale. The conversion specifier is `f` or `e` (resolving in favor of `f` in
   case of a tie), chosen according to the requirement for a shortest
   representation: the string representation consists of the smallest number of
   characters such that there is at least one digit before the radix point (if
   present) and parsing the representation using the corresponding
   `std::from_chars` function recovers value exactly. If there are several such
   representations, one with the smallest difference to `value` is chosen,
   resolving any remaining ties using rounding according to
   `std::round_to_nearest`. The library provides overloads for all
   cv-unqualified floating-point types as the type of the parameter
   `value`.(since C++23)

4) Same as (3), but the conversion specified for the as-if printf is `f` if
   `fmt` is `std::chars_format::fixed`, `e` if `fmt` is
   `std::chars_format::scientific`, `a` (but without leading "0x" in the result)
   if `fmt` is `std::chars_format::hex`, and `g` if `fmt` is
   `chars_format::general`. The library provides overloads for all
   cv-unqualified floating-point types as the type of the parameter
   `value`.(since C++23)

5) Same as (4), except the precision is specified by the parameter `precision`
   rather than by the shortest representation requirement. The library provides
   overloads for all cv-unqualified floating-point types as the type of the
   parameter `value`.(since C++23)

### Parameters

- **first, last** — character range to write to
- **value** — the value to convert to its string representation
- **base** — integer base to use: a value between 2 and 36 (inclusive).
- **fmt** — floating-point formatting to use, a bitmask of type
  `std::chars_format`
- **precision** — floating-point precision to use

### Return value

On success, returns a value of type `std::to_chars_result` such that `ec` equals
value-initialized `std::errc` and `ptr` is the one-past-the-end pointer of the
characters written. Note that the string is *not* NUL-terminated.

On error, returns a value of type `std::to_chars_result` holding
`std::errc::value_too_large` in `ec`, a copy of the value `last` in `ptr`, and
leaves the contents of the range `[``first``,``last``)` in unspecified state.

### Exceptions

Throws nothing.

### Notes

Unlike other formatting functions in C++ and C libraries, `std::to_chars` is
locale-independent, non-allocating, and non-throwing. Only a small subset of
formatting policies used by other libraries (such as `std::sprintf`) is
provided. This is intended to allow the fastest possible implementation that is
useful in common high-throughput contexts such as text-based interchange (JSON
or XML).

The guarantee that `std::from_chars` can recover every floating-point value
formatted by `std::to_chars` exactly is only provided if both functions are from
the same implementation.

It is required to explicitly cast a bool value to another integer type if it is
wanted to format the value as `"0"`/`"1"`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_to_chars` | 201611L | (C++17) | Elementary string conversions
      (`std::to_chars`, `std::from_chars`)
  202306L | (C++26) | Testing for success or failure of `<charconv>` functions
  `__cpp_lib_constexpr_charconv` | 202207L | (C++23) | Add constexpr modifiers
      to `std::to_chars` and `std::from_chars` overloads (1) for integral types

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

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2955 | C++17 | this function was in `<utility>` and used `std::error_code`
      | moved to `<charconv>` and uses `std::errc`
  LWG 3266 | C++17 | bool argument was accepted and promoted to int | rejected
      by a deleted overload
  LWG 3373 | C++17 | `std::to_chars_result` might have additional members |
      additional members are disallowed

### See also

- **to_chars_result (C++17)** — the return type of **`std::to_chars`** (class)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)
- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)
- **operator<<** — inserts formatted data (public member function of
  `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/to_chars*
