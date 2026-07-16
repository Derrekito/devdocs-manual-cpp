# std::stoul, std::stoull

```cpp
unsigned long      stoul ( const std::string& str,
                           std::size_t* pos = nullptr, int base = 10 );  // (1) (since C++11)
unsigned long      stoul ( const std::wstring& str,
                           std::size_t* pos = nullptr, int base = 10 );  // (2) (since C++11)
unsigned long long stoull( const std::string& str,
                           std::size_t* pos = nullptr, int base = 10 );  // (3) (since C++11)
unsigned long long stoull( const std::wstring& str,
                           std::size_t* pos = nullptr, int base = 10 );  // (4) (since C++11)
```

Interprets an unsigned integer value in the string `str`.

Let `ptr` be an internal (to the conversion functions) pointer of type char*
(1,3) or wchar_t* (2,4), accordingly.

1) Calls `std::strtoul(str.c_str(), &ptr, base)`.

2) Calls `std::wcstoul(str.c_str(), &ptr, base)`.

3) Calls `std::strtoull(str.c_str(), &ptr, base)`.

4) Calls `std::wcstoull(str.c_str(), &ptr, base)`.

Discards any whitespace characters (as identified by calling `std::isspace`)
until the first non-whitespace character is found, then takes as many characters
as possible to form a valid *base-n* (where n=`base`) unsigned integer number
representation and converts them to an integer value. The valid unsigned integer
value consists of the following parts:

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
from the sequence of digits is negated as if by unary minus in the result type,
which applies unsigned integer wraparound rules.

If `pos` is not a null pointer, then `ptr` will receive the address of the first
unconverted character in `str.c_str()`, and the index of that character will be
calculated and stored in `*pos`, giving the number of characters that were
processed by the conversion.

### Parameters

- **str** — the string to convert
- **pos** — address of an integer to store the number of characters processed
- **base** — the number base

### Return value

The string converted to the specified unsigned integer type.

### Exceptions

- `std::invalid_argument` if no conversion could be performed.
- `std::out_of_range` if the converted value would fall out of the range of the
  result type or if the underlying function (`std::strtoul` or `std::strtoull`)
  sets `errno` to `ERANGE`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2009 | C++11 | `std::out_of_range` would not be thrown if `std::strtoul`
      or `std::strtoull` sets `errno` to `ERANGE` | will throw

### See also

- **stoistolstoll (C++11)(C++11)(C++11)** — converts a string to a signed
  integer (function)
- **stofstodstold (C++11)(C++11)(C++11)** — converts a string to a floating
  point value (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/stoul*
