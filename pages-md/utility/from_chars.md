# std::from_chars

Parses a character sequence into a number, in place. Unlike
`std::strtol`/`std::sscanf`/`std::stoi`, `std::from_chars` is
locale-independent, non-allocating, and non-throwing — it exists to
be the fastest possible conversion for high-throughput text formats
like JSON or XML, at the cost of the small, strict grammar described
below. Success or failure comes back as a `std::errc` in the returned
`std::from_chars_result`, not an exception.

```cpp skip
std::from_chars(first, last, int_value);           // integer, base 10
std::from_chars(first, last, int_value, base);      // integer, base 2..36
std::from_chars(first, last, double_value);         // float, chars_format::general
std::from_chars(first, last, double_value, fmt);    // float, explicit chars_format
```

Integer overloads are `constexpr` since C++23.

### What you provide

- **first, last** — `const char*` range to parse; need not be
  NUL-terminated.
- **value** — an out-parameter (integer or floating-point reference)
  where the parsed value is stored on success; left unmodified on
  failure.
- **base** — integer base, 2 to 36 inclusive (default 10). `"0x"`/`"0X"`
  prefixes are never recognized, even for base 16.
- **fmt** — a `std::chars_format` bitmask (`general`, `fixed`,
  `scientific`, `hex`) controlling which floating-point grammar to
  accept.

### Guarantees and costs

- Throws nothing; failure is reported through `ec`, never an
  exception.
- Locale-independent: parsing always follows the default ("C") locale
  grammar, regardless of the program's current locale.
- Integers follow `std::strtol`'s grammar except: no `0x`/`0X` prefix
  for base 16, only a minus sign is recognized (and only for signed
  types), and leading whitespace is not skipped.
- Floats follow `std::strtod`'s grammar (adjusted per `fmt`), also
  without a plus sign or leading-whitespace skipping; the result is
  the nearest representable value, rounding ties per
  `std::round_to_nearest`.
- `from_chars` is guaranteed to recover exactly what `to_chars` wrote
  only when both come from the same standard library implementation.

### Gotchas

- Always check `ec` — a failed call leaves `value` untouched, so
  reading it without checking `ec` first silently uses stale data.
- No match at all (e.g. non-numeric input) gives
  `std::errc::invalid_argument` with `ptr == first`.
- A match that doesn't fit the target type gives
  `std::errc::result_out_of_range`, with `ptr` past the matched
  characters but `value` still unmodified.
- Leading whitespace is **not** skipped — `" 42"` fails to parse where
  `strtol`/`sscanf` would succeed. A lone sign with no digits after it
  also counts as no match.

### Example

```cpp c++17
#include <charconv>
#include <iomanip>
#include <iostream>
#include <string_view>
#include <system_error>

int main()
{
    for (std::string_view str : {"1234", "15 foo", "bar", " 42"})
    {
        int result{};
        auto [ptr, ec] =
            std::from_chars(str.data(), str.data() + str.size(), result);

        std::cout << std::quoted(str) << ": ";
        if (ec == std::errc())
            std::cout << "value " << result
                       << ", remainder " << std::quoted(ptr) << '\n';
        else if (ec == std::errc::invalid_argument)
            std::cout << "not a number\n";
        else
            std::cout << "out of range\n";
    }
}
```

```text
"1234": value 1234, remainder ""
"15 foo": value 15, remainder " foo"
"bar": not a number
" 42": not a number
```

### Reference

```cpp skip
std::from_chars_result
    from_chars( const char* first, const char* last,
                /* integer-type */& value, int base = 10 );  // (since C++17)
std::from_chars_result
    from_chars( const char* first, const char* last, float& value,
                std::chars_format fmt = std::chars_format::general );
std::from_chars_result
    from_chars( const char* first, const char* last, double& value,
                std::chars_format fmt = std::chars_format::general );
std::from_chars_result
    from_chars( const char* first, const char* last, long double& value,
                std::chars_format fmt = std::chars_format::general );
```

Formally: on success, `ptr` is the first non-matching character (or
`last`, if the whole range matched) and `ec` is value-initialized. On
no match, `ptr == first` and `ec == std::errc::invalid_argument`. On a
match that overflows the target type, `ec ==
std::errc::result_out_of_range`. `from_chars` provides overloads for
all cv-unqualified signed and unsigned integer types plus `char`, and
for all cv-unqualified floating-point types.

Feature-test macros: `__cpp_lib_to_chars` — `201611L` (C++17, the
`from_chars`/`to_chars` family), `202306L` (C++26, `operator bool()` on
the result for success/failure testing); `__cpp_lib_constexpr_charconv`
— `202207L` (C++23, `constexpr` for the integer overloads).

### See also

- **from_chars_result (C++17)** — the return type of `std::from_chars`
- **to_chars (C++17)** — the inverse: number to character sequence
- **to_string (C++11)** — simpler, locale-sensitive numeric-to-string conversion
- **stoi, stol, stoll (C++11)** — string to signed integer, throws on failure
- **stof, stod, stold (C++11)** — string to floating point, throws on failure
- **operator>>** — extracts formatted data (of `std::basic_istream`)

---
*Source: https://en.cppreference.com/w/cpp/utility/from_chars*
