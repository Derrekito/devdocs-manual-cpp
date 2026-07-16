# std::to_chars

Converts a number into a character sequence, writing into a buffer you
already own. Like `std::from_chars`, it exists because
`std::sprintf`/`std::to_string` are locale-dependent, allocate, and
are comparatively slow: `to_chars` is locale-independent,
non-allocating, and non-throwing — the fastest conversion available,
meant for high-throughput text formats like JSON or XML. Failure comes
back as a `std::errc` in the returned `std::to_chars_result`, not an
exception.

```cpp skip
std::to_chars(first, last, int_value);                    // integer, base 10
std::to_chars(first, last, int_value, base);               // integer, base 2..36
std::to_chars(first, last, double_value);                  // float, shortest round-trip
std::to_chars(first, last, double_value, fmt);              // float, explicit chars_format
std::to_chars(first, last, double_value, fmt, precision);   // float, explicit precision
```

Integer overloads are `constexpr` since C++23. The `bool` overload is
explicitly deleted — `to_chars` refuses to silently print `"0"`/`"1"`
for a `bool`; cast to another integer type if that's what you want.

### What you provide

- **first, last** — `char*` range to write into; must be a valid
  range large enough for the result, since `to_chars` never allocates.
- **value** — the number to convert (integer, `float`, `double`, or
  `long double`).
- **base** — integer base, 2 to 36 inclusive (default 10); digits 10–35
  are written as lowercase `a`–`z`.
- **fmt** — a `std::chars_format` selecting `f`-style (`fixed`),
  `e`-style (`scientific`), `a`-style without the `0x` prefix (`hex`),
  or `g`-style (`general`) formatting, as if by `std::printf`.
- **precision** — digits after the radix point, when you don't want
  the default shortest round-trippable representation.

### Guarantees and costs

- Throws nothing; failure is reported through `ec`, never an
  exception.
- Locale-independent and non-allocating: writes only into
  `[first, last)`, never touches the heap.
- With no `precision` given, the float overloads pick the *shortest*
  representation that round-trips exactly back through the matching
  `from_chars` call — guaranteed only when both functions come from
  the same standard library implementation.
- The written characters are **not** NUL-terminated.

### Gotchas

- Always check `ec` — on failure the buffer's contents are left in an
  unspecified state, not a partial-but-usable string.
- A buffer too small to hold the result gives
  `std::errc::value_too_large`, with `ptr` set to `last`.
- Passing a `bool` doesn't compile — the overload is deleted precisely
  to stop you from getting `"0"`/`"1"` where you meant `"false"`/`"true"`.

### Example

```cpp c++17
#include <array>
#include <charconv>
#include <iostream>
#include <string_view>
#include <system_error>

void show(auto... format_args)
{
    std::array<char, 16> buf;
    auto [ptr, ec] =
        std::to_chars(buf.data(), buf.data() + buf.size(), format_args...);

    if (ec == std::errc())
        std::cout << std::string_view(buf.data(), ptr - buf.data()) << '\n';
    else
        std::cout << "buffer too small\n";
}

int main()
{
    show(42);
    show(3.14159F);
    show(-3.14159, std::chars_format::fixed);
    show(-3.14159, std::chars_format::scientific, 3);
}
```

```text
42
3.14159
-3.14159
-3.142e+00
```

### Reference

```cpp skip
std::to_chars_result
    to_chars( char* first, char* last,
              /* integer-type */ value, int base = 10 );  // (since C++17)
std::to_chars_result
    to_chars( char*, char*, bool, int = 10 ) = delete;
std::to_chars_result
    to_chars( char* first, char* last, float value );
std::to_chars_result
    to_chars( char* first, char* last, double value );
std::to_chars_result
    to_chars( char* first, char* last, long double value );
std::to_chars_result
    to_chars( char* first, char* last, float value, std::chars_format fmt );
std::to_chars_result
    to_chars( char* first, char* last, double value, std::chars_format fmt );
std::to_chars_result
    to_chars( char* first, char* last, long double value,
              std::chars_format fmt );
std::to_chars_result
    to_chars( char* first, char* last, float value,
              std::chars_format fmt, int precision );
std::to_chars_result
    to_chars( char* first, char* last, double value,
              std::chars_format fmt, int precision );
std::to_chars_result
    to_chars( char* first, char* last, long double value,
              std::chars_format fmt, int precision );
```

Formally: on success, `ec` is value-initialized and `ptr` is the
one-past-the-end pointer of the written characters. On failure, `ec ==
std::errc::value_too_large` and `ptr == last`. Integer overloads write
no redundant leading zeroes and a leading `-` for negative values. The
library provides overloads for all cv-unqualified signed and unsigned
integer types plus `char`, and for all cv-unqualified floating-point
types.

Feature-test macros: `__cpp_lib_to_chars` — `201611L` (C++17, the
`to_chars`/`from_chars` family), `202306L` (C++26, `operator bool()` on
the result); `__cpp_lib_constexpr_charconv` — `202207L` (C++23,
`constexpr` for the integer overload).

### See also

- **to_chars_result (C++17)** — the return type of `std::to_chars`
- **from_chars (C++17)** — the inverse: character sequence to number
- **to_string (C++11)** — simpler, locale-sensitive numeric-to-string conversion
- **printf, sprintf, snprintf (C++11)** — formatted output to a buffer or stream
- **operator<<** — inserts formatted data (of `std::basic_ostream`)

---
*Source: https://en.cppreference.com/w/cpp/utility/to_chars*
