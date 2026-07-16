# std::stof, std::stod, std::stold

```cpp
float       stof ( const std::string& str, std::size_t* pos = nullptr );  // (1) (since C++11)
float       stof ( const std::wstring& str, std::size_t* pos = nullptr );  // (2) (since C++11)
double      stod ( const std::string& str, std::size_t* pos = nullptr );  // (3) (since C++11)
double      stod ( const std::wstring& str, std::size_t* pos = nullptr );  // (4) (since C++11)
long double stold( const std::string& str, std::size_t* pos = nullptr );  // (5) (since C++11)
long double stold( const std::wstring& str, std::size_t* pos = nullptr );  // (6) (since C++11)
```

Interprets a floating point value in a string `str`.

Let `ptr` be an internal (to the conversion functions) pointer of type char*
(1,3,5) or wchar_t* (2,4,6), accordingly.

1) Calls `std::strtof(str.c_str(), &ptr)`.

2) Calls `std::wcstof(str.c_str(), &ptr)`.

3) Calls `std::strtod(str.c_str(), &ptr)`.

4) Calls `std::wcstod(str.c_str(), &ptr)`.

5) Calls `std::strtold(str.c_str(), &ptr)`.

6) Calls `std::wcstold(str.c_str(), &ptr)`.

Function discards any whitespace characters (as determined by `std::isspace`)
until first non-whitespace character is found. Then it takes as many characters
as possible to form a valid floating-point representation and converts them to a
floating-point value. The valid floating-point value can be one of the
following:

- decimal floating-point expression. It consists of the following parts:
- hexadecimal floating-point expression. It consists of the following parts:
- infinity expression. It consists of the following parts:
- not-a-number expression. It consists of the following parts:
- any other expression that may be accepted by the currently installed C locale

If `pos` is not a null pointer, then `ptr` will receive the address of the first
unconverted character in `str.c_str()`, and the index of that character will be
calculated and stored in `*pos`, giving the number of characters that were
processed by the conversion.

### Parameters

- **str** — the string to convert
- **pos** — address of an integer to store the number of characters processed

### Return value

The string converted to the specified floating point type.

### Exceptions

`std::invalid_argument` if no conversion could be performed.

`std::out_of_range` if the converted value would fall out of the range of the
result type or if the underlying function (`std::strtof`, `std::strtod` or
`std::strtold`) sets `errno` to `ERANGE`.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2009 | C++11 | `std::out_of_range` would not be thrown if the converted
      value would fall out of the range of the result type | will throw
  LWG 2403 | C++11 | `stof` called `std::strtod` or `std::wcstod` | `stof` calls
      `std::strtof` or `std::wcstof`

### See also

- **stoistolstoll (C++11)(C++11)(C++11)** — converts a string to a signed
  integer (function)
- **stoulstoull (C++11)(C++11)** — converts a string to an unsigned integer
  (function)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/stof*
