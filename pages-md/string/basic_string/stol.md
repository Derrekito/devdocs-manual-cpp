# std::stoi, std::stol, std::stoll

`std::stoi`, `std::stol`, and `std::stoll` (all C++11) do the same
job — parse a signed integer from the front of a `std::string` (or
`std::wstring`) — and differ only in the width of the result (`int`,
`long`, `long long`). Unlike C's `atoi`/`strtol`, they signal bad
input by throwing rather than returning a sentinel value:
`std::invalid_argument` if nothing could be parsed at all,
`std::out_of_range` if the value doesn't fit the result type.

```cpp skip
int       n  = std::stoi(s);          // (since C++11)
long      l  = std::stol(s);          // (since C++11)
long long ll = std::stoll(s);         // (since C++11)
std::stoi(s, &pos);                   // pos <- index where parsing stopped
std::stoi(s, nullptr, base);          // parse in a given base (0 = auto-detect)
```

### What you provide

- **str** — the string to parse (`std::string` selects the narrow
  overloads, `std::wstring` the wide ones). Leading whitespace is
  skipped automatically.
- **pos** (optional, default `nullptr`) — if non-null, receives the
  index of the first unconverted character: how many characters were
  actually consumed.
- **base** (optional, default `10`) — the numeric base, any of
  `2`-`36`, or `0` to auto-detect a `0x`/`0X` (hex) or `0` (octal)
  prefix.

### Guarantees and costs

- Implemented in terms of `std::strtol`/`std::wcstol` (`stoi`,
  `stol`) or `std::strtoll`/`std::wcstoll` (`stoll`) — same parsing
  rules apply: an optional sign, an optional base prefix, then digits;
  parsing stops at the first character that doesn't fit.
- A "valid" value is more than plain digits: leading whitespace, an
  optional `+`/`-`, and (for base `0`, `8`, or `16`) a `0`/`0x` prefix
  are all part of what gets consumed.
- Additional numeric formats may be accepted depending on the
  currently installed C locale.

### Gotchas

- These throw, they don't return an error code:
  `std::invalid_argument` when no conversion happened at all,
  `std::out_of_range` when the value is too large/small for the result
  type (including when the underlying `strtol`/`strtoll` sets `errno`
  to `ERANGE`). Untrusted input needs a `try`/`catch`.
- Partial parses succeed silently — `"31337 with words"` parses as
  `31337` with no error. Check the `pos` out-parameter if trailing
  junk after the number should itself be treated as invalid.
- Passing a `std::wstring` selects the wide overloads automatically;
  there's no implicit conversion between `std::string` and
  `std::wstring`, so the two families can't be mixed in one call.

### Example

```cpp
#include <iostream>
#include <stdexcept>
#include <string>

int main()
{
    for (const std::string s :
         {"45", "3.14159", "31337 with words", "12345678901"})
    {
        try
        {
            std::size_t pos{};
            int n = std::stoi(s, &pos);
            std::cout << s << " -> " << n << " (pos " << pos << ")\n";
        }
        catch (const std::invalid_argument&)
        {
            std::cout << s << " -> invalid_argument\n";
        }
        catch (const std::out_of_range&)
        {
            std::cout << s << " -> out_of_range\n";
        }
    }
}
```

```text
45 -> 45 (pos 2)
3.14159 -> 3 (pos 1)
31337 with words -> 31337 (pos 5)
12345678901 -> out_of_range
```

### Reference

```cpp skip
int       stoi ( const std::string& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (1) (since C++11)
int       stoi ( const std::wstring& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (2) (since C++11)
long      stol ( const std::string& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (3) (since C++11)
long      stol ( const std::wstring& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (4) (since C++11)
long long stoll( const std::string& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (5) (since C++11)
long long stoll( const std::wstring& str,
                 std::size_t* pos = nullptr, int base = 10 );  // (6) (since C++11)
```

Formally: (1,3,5) call `std::strtol`/`std::strtol`/`std::strtoll` on
`str.c_str()`; (2,4,6) call the wide `std::wcstol`/`std::wcstol`/
`std::wcstoll` equivalents. Valid bases are `0` and `2`-`36`; digits
above `9` use letters (`A`/`a` = 10 through `Z`/`z` = 35),
case-insensitive. If the minus sign was present, the value is negated
as if by unary minus in the result type. Defect report LWG 2009
(applied to C++11) added the requirement that `std::out_of_range` be
thrown when the underlying `strtol`/`strtoll` call sets `errno` to
`ERANGE` — earlier wording left this unspecified.

### See also

- **stoul / stoull** (C++11) — same, but for unsigned integers
- **stof / stod / stold** (C++11) — same, but for floating-point values
- **from_chars** (C++17) — non-throwing, non-allocating alternative
- **to_string / to_wstring** (C++11) — the reverse direction: number to
  string

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/stol*
