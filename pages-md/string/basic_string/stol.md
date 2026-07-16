# std::stoi, std::stol, std::stoll

```cpp
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

Interprets a signed integer value in the string `str`.

Let `ptr` be an internal (to the conversion functions) pointer of type char*
(1,3,5) or wchar_t* (2,4,6), accordingly.

1) Calls `std::strtol(str.c_str(), &ptr, base)`.

2) Calls `std::wcstol(str.c_str(), &ptr, base)`.

3) Calls `std::strtol(str.c_str(), &ptr, base)`.

4) Calls `std::wcstol(str.c_str(), &ptr, base)`.

5) Calls `std::strtoll(str.c_str(), &ptr, base)`.

6) Calls `std::wcstoll(str.c_str(), &ptr, base)`.

Discards any whitespace characters (as identified by calling `std::isspace`)
until the first non-whitespace character is found, then takes as many characters
as possible to form a valid *base-n* (where n=`base`) integer number
representation and converts them to an integer value. The valid integer value
consists of the following parts:

- (optional) plus or minus sign
- (optional) prefix (`0`) indicating octal base (applies only when the base is
  `8` or `​0​`)
- (optional) prefix (`0x` or `0X`) indicating hexadecimal base (applies only
  when the base is `16` or `​0​`)
- a sequence of digits

The set of valid values for base is `{0,2,3,...,36}.` The set of valid digits
for base-`2` integers is `{0,1},` for base-`3` integers is `{0,1,2},` and so on.
For bases larger than `10`, valid digits include alphabetic characters, starting
from `Aa` for base-`11` integer, to `Zz` for base-`36` integer. The case of the
characters is ignored.

Additional numeric formats may be accepted by the currently installed C locale.

If the value of `base` is `​0​`, the numeric base is auto-detected: if the
prefix is `0`, the base is octal, if the prefix is `0x` or `0X`, the base is
hexadecimal, otherwise the base is decimal.

If the minus sign was part of the input sequence, the numeric value calculated
from the sequence of digits is negated as if by unary minus in the result type.

If `pos` is not a null pointer, then `ptr` will receive an address of the first
unconverted character in `str.c_str()`, and the index of that character will be
calculated and stored in `*pos`, giving the number of characters that were
processed by the conversion.

### Parameters

- **str** — the string to convert
- **pos** — address of an integer to store the number of characters processed
- **base** — the number base

### Return value

Integer value corresponding to the content of `str`.

### Exceptions

- `std::invalid_argument` if no conversion could be performed.
- `std::out_of_range` if the converted value would fall out of the range of the
  result type or if the underlying function (`std::strtol` or `std::strtoll`)
  sets `errno` to `ERANGE`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <stdexcept>
#include <string>
#include <utility>

int main()
{
    const auto data =
    {
        "45",
        "+45",
        " -45",
        "3.14159",
        "31337 with words",
        "words and 2",
        "12345678901",
    };

    for (const std::string s : data)
    {
        std::size_t pos{};
        try
        {
            std::cout << "std::stoi(" << std::quoted(s) << "): ";
            const int i{std::stoi(s, &pos)};
            std::cout << i << "; pos: " << pos << '\n';
        }
        catch (std::invalid_argument const& ex)
        {
            std::cout << "std::invalid_argument::what(): " << ex.what() << '\n';
        }
        catch (std::out_of_range const& ex)
        {
            std::cout << "std::out_of_range::what(): " << ex.what() << '\n';
            const long long ll{std::stoll(s, &pos)};
            std::cout << "std::stoll(" << std::quoted(s) << "): " << ll
                      << "; pos: " << pos << '\n';
        }
    }

    std::cout << "\nCalling with different radixes:\n";
    for (const auto& [s, base] : {std::pair<const char*, int>
        {"11",  2}, {"22",  3}, {"33",  4}, {"77",  8},
        {"99", 10}, {"FF", 16}, {"jJ", 20}, {"Zz", 36}})
    {
        const int i{std::stoi(s, nullptr, base)};
        std::cout << "std::stoi(" << std::quoted(s)
                  << ", nullptr, " << base << "): " << i << '\n';
    }
}
```

Possible output:

```text
std::stoi("45"): 45; pos: 2
std::stoi("+45"): 45; pos: 3
std::stoi(" -45"): -45; pos: 4
std::stoi("3.14159"): 3; pos: 1
std::stoi("31337 with words"): 31337; pos: 5
std::stoi("words and 2"): std::invalid_argument::what(): stoi
std::stoi("12345678901"): std::out_of_range::what(): stoi
std::stoll("12345678901"): 12345678901; pos: 11

Calling with different radixes:
std::stoi("11", nullptr, 2): 3
std::stoi("22", nullptr, 3): 8
std::stoi("33", nullptr, 4): 15
std::stoi("77", nullptr, 8): 63
std::stoi("99", nullptr, 10): 99
std::stoi("FF", nullptr, 16): 255
std::stoi("jJ", nullptr, 20): 399
std::stoi("Zz", nullptr, 36): 1295
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2009 | C++11 | `std::out_of_range` would not be thrown if `std::strtol` or
      `std::strtoll` sets `errno` to `ERANGE` | will throw

### See also

- **stoulstoull (C++11)(C++11)** — converts a string to an unsigned integer
  (function)
- **stofstodstold (C++11)(C++11)(C++11)** — converts a string to a floating
  point value (function)
- **strtolstrtoll (C++11)** — converts a byte string to an integer value
  (function)
- **strtoulstrtoull (C++11)** — converts a byte string to an unsigned integer
  value (function)
- **strtoimaxstrtoumax (C++11)(C++11)** — converts a byte string to
  `std::intmax_t` or `std::uintmax_t` (function)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **atoiatolatoll (C++11)** — converts a byte string to an integer value
  (function)
- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)
- **to_wstring (C++11)** — converts an integral or floating-point value to
  `wstring` (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/stol*
