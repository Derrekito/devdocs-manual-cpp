# std::strtoimax, std::strtoumax

```cpp
std::intmax_t strtoimax( const char* nptr, char** endptr, int base );  // (1) (since C++11)
std::uintmax_t strtoumax( const char* nptr, char** endptr, int base );  // (2) (since C++11)
```

Interprets an integer value in a byte string pointed to by `nptr`.

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

The functions sets the pointer pointed to by `endptr` to point to the character
past the last character interpreted. If `endptr` is a null pointer, it is
ignored.

If the `nptr` is empty or does not have the expected form, no conversion is
performed, and (if `enptr` is not a null pointer) the value of `nptr` is stored
in the object pointed to by `endptr`.

### Parameters

- **nptr** — pointer to the null-terminated byte string to be interpreted
- **endptr** — pointer to a pointer to character.
- **base** — *base* of the interpreted integer value

### Return value

- If successful, an integer value corresponding to the contents of `str` is
  returned.
- If the converted value falls out of range of corresponding return type, a
  range error occurs (setting `errno` to `ERANGE`) and `INTMAX_MAX`,
  `INTMAX_MIN`, `UINTMAX_MAX` or `​0​` is returned, as appropriate.
- If no conversion can be performed, `​0​` is returned.

### Example

```cpp
#include <cinttypes>
#include <iostream>
#include <string>

int main()
{
    std::string str = "helloworld";
    std::intmax_t val = std::strtoimax(str.c_str(), nullptr, 36);
    std::cout << str << " in base 36 is " << val << " in base 10\n";

    char* nptr;
    val = std::strtoimax(str.c_str(), &nptr, 30);
    if (nptr != &str[0] + str.size())
        std::cout << str << " in base 30 is invalid."
                  << " The first invalid digit is '" << *nptr << "'\n";
}
```

Output:

```text
helloworld in base 36 is 1767707668033969 in base 10
helloworld in base 30 is invalid. The first invalid digit is 'w'
```

### See also

- **stoistolstoll (C++11)(C++11)(C++11)** — converts a string to a signed
  integer (function)
- **stoulstoull (C++11)(C++11)** — converts a string to an unsigned integer
  (function)
- **strtolstrtoll (C++11)** — converts a byte string to an integer value
  (function)
- **strtoulstrtoull (C++11)** — converts a byte string to an unsigned integer
  value (function)
- **wcstoimaxwcstoumax (C++11)(C++11)** — converts a wide string to
  `std::intmax_t` or `std::uintmax_t` (function)
- **strtofstrtodstrtold** — converts a byte string to a floating-point value
  (function)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **atoiatolatoll (C++11)** — converts a byte string to an integer value
  (function)

**C documentation for `strtoimax, strtoumax`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strtoimax*
