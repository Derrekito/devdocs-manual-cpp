# std::formatter

```cpp
template< class T, class CharT = char >
struct formatter;  // (since C++20)
```

The enabled specializations of `std::formatter` define formatting rules for a
given type. Enabled specializations meet the BasicFormatter requirements, and,
unless otherwise specified, also meet the Formatter requirements.

For all types `T` and `CharT` for which no specialization `std::formatter<T,
CharT>` is enabled, that specialization is a complete type and is disabled.

Disabled specializations do not meet the Formatter requirements, and the
following are all `false`:

- `std::is_default_constructible_v`
- `std::is_copy_constructible_v`
- `std::is_move_constructible_v`
- `std::is_copy_assignable_v`
- `std::is_move_assignable_v`.

### Basic standard specializations

In the following list, `CharT` is either char or wchar_t, `ArithmeticT` is any
cv-unqualified arithmetic type other than char, wchar_t, char8_t, char16_t, or
char32_t:

```cpp
Character formatters
template<>
struct formatter<char, char>;  // (1)
template<>
struct formatter<char, wchar_t>;  // (2)
template<>
struct formatter<wchar_t, wchar_t>;  // (3)
String formatters
template<>
struct formatter<CharT*, CharT>;  // (4)
template<>
struct formatter<const CharT*, CharT>;  // (5)
template< std::size_t N >
struct formatter<CharT[N], CharT>;  // (6)
template< std::size_t N >
struct formatter<const CharT[N], CharT>;  // (7) (until C++23)
template< class Traits, class Alloc >
struct formatter<std::basic_string<CharT, Traits, Alloc>, CharT>;  // (8)
template< class Traits >
struct formatter<std::basic_string_view<CharT, Traits>, CharT>;  // (9)
Arithmetic formatters
template<>
struct formatter<ArithmeticT, CharT>;  // (10)
Pointer formatters
template<>
struct formatter<std::nullptr_t, CharT>;  // (11)
template<>
struct formatter<void*, CharT>;  // (12)
template<>
struct formatter<const void*, CharT>;  // (13)
```

Formatters for other pointers and pointers to members are disabled.

Specializations such as std::formatter<wchar_t, char> and std::formatter<const
char*, wchar_t> that would require encoding conversions are disabled.

Each formatter specialization for string or character type additionally provides
a public non-static member function `constexpr void set_debug_format();` which
modifies the state of the formatter object so that it will format the values as
escaped and quoted, as if the type of the format specifier parsed by the last
call to `parse` were `?`.
*(since C++23)*

### Standard format specification

For basic types and string types, the format specification is based on the
format specification in Python.

The syntax of format specifications is:

```text
fill-and-align ﻿(optional) sign ﻿(optional) #(optional) 0(optional) width ﻿(optional) precision ﻿(optional) L(optional) type ﻿(optional)
```

The sign, `#` and `0` options are only valid when an integer or floating-point
presentation type is used.

In most of the cases the syntax is similar to the old `%`-formatting, with the
addition of the `{}` and with `:` used instead of `%`. For example, '%03.2f' can
be translated to '{:03.2f}'.

##### Fill and align

fill-and-align is an optional *fill* character (which can be any character other
than `{` or `}`), followed by one of the *align* options `<`, `>`, `^`.

If no fill character is specified, it defaults to the space character. For a
format specification in a Unicode encoding, the fill character must correspond
to a single Unicode scalar value.

The meaning of *align* options is as follows:

- `<`: Forces the formatted argument to be aligned to the start of the available
  space by inserting n fill characters after the formatted argument. This is the
  default when a non-integer non-floating-point presentation type is used.
- `>`: Forces the formatted argument to be aligned to the end of the available
  space by inserting n fill characters before the formatted argument. This is
  the default when an integer or floating-point presentation type is used.
- `^`: Forces the formatted argument to be centered within the available space
  by inserting ⌊n/2⌋ characters before and ⌈n/2⌉ characters after the formatted
  argument.

In each case, n is the difference of the minimum field width (specified by
width) and the estimated width of the formatted argument, or 0 if the difference
is less than 0.

```cpp
char c = 120;
auto s0 = std::format("{:6}", 42);    // value of s0 is "    42"
auto s1 = std::format("{:6}", 'x');   // value of s1 is "x     "
auto s2 = std::format("{:*<6}", 'x'); // value of s2 is "x*****"
auto s3 = std::format("{:*>6}", 'x'); // value of s3 is "*****x"
auto s4 = std::format("{:*^6}", 'x'); // value of s4 is "**x***"
auto s5 = std::format("{:6d}", c);    // value of s5 is "   120"
auto s6 = std::format("{:6}", true);  // value of s6 is "true  "
```

##### Sign, #, and 0

The sign option can be one of following:

- `+`: Indicates that a sign should be used for both non-negative and negative
  numbers. The `+` sign is inserted before the output value for non-negative
  numbers.
- `-`: Indicates that a sign should be used for negative numbers only (this is
  the default behavior).
- space: Indicates that a leading space should be used for non-negative numbers,
  and a minus sign for negative numbers.

Negative zero is treated as a negative number.

The sign option applies to floating-point infinity and NaN.

```cpp
double inf = std::numeric_limits<double>::infinity();
double nan = std::numeric_limits<double>::quiet_NaN();
auto s0 = std::format("{0:},{0:+},{0:-},{0: }", 1);   // value of s0 is "1,+1,1, 1"
auto s1 = std::format("{0:},{0:+},{0:-},{0: }", -1);  // value of s1 is "-1,-1,-1,-1"
auto s2 = std::format("{0:},{0:+},{0:-},{0: }", inf); // value of s2 is "inf,+inf,inf, inf"
auto s3 = std::format("{0:},{0:+},{0:-},{0: }", nan); // value of s3 is "nan,+nan,nan, nan"
```

The `#` option causes the *alternate form* to be used for the conversion.

- For integral types, when binary, octal, or hexadecimal presentation type is
  used, the alternate form inserts the prefix (`0b`, `0`, or `0x`) into the
  output value after the sign character (possibly space) if there is one, or add
  it before the output value otherwise.
- For floating-point types, the alternate form causes the result of the
  conversion of finite values to always contain a decimal-point character, even
  if no digits follow it. Normally, a decimal-point character appears in the
  result of these conversions only if a digit follows it. In addition, for `g`
  and `G` conversions, trailing zeros are not removed from the result.

The `0` option pads the field with leading zeros (following any indication of
sign or base) to the field width, except when applied to an infinity or NaN. If
the `0` character and an *align* option both appear, the `0` character is
ignored.

```cpp
char c = 120;
auto s1 = std::format("{:+06d}", c);   // value of s1 is "+00120"
auto s2 = std::format("{:#06x}", 0xa); // value of s2 is "0x000a"
auto s3 = std::format("{:<06}", -42);  // value of s3 is "-42   "
                                       // (0 is ignored because of < alignment)
```

##### Width and precision

width is either a positive decimal number, or a nested replacement field (`{}`
or `{`*n*`}`). If present, it specifies the minimum field width.

precision is a dot (`.`) followed by either a non-negative decimal number or a
nested replacement field. This field indicates the precision or maximum field
size. It can only be used with floating-point and string types.

- For floating-point types, this field specifies the formatting precision.
- For string types, it provides an upper bound for the estimated width (see
  below) of the prefix of the string to be copied to the output. For a string in
  a Unicode encoding, the text to be copied to the output is the longest prefix
  of whole extended grapheme clusters whose estimated width is no greater than
  the precision.

If a nested replacement field is used for width or precision, and the
corresponding argument is not of integral type(until C++23)standard signed or
unsigned integer type(since C++23), or is negative, an exception of type
`std::format_error` is thrown.

```cpp
float pi = 3.14f;
auto s1 = std::format("{:10f}", pi);           // s1 = "  3.140000" (width = 10)
auto s2 = std::format("{:{}f}", pi, 10);       // s2 = "  3.140000" (width = 10)
auto s3 = std::format("{:.5f}", pi);           // s3 = "3.14000" (precision = 5)
auto s4 = std::format("{:.{}f}", pi, 5);       // s4 = "3.14000" (precision = 5)
auto s5 = std::format("{:10.5f}", pi);         // s5 = "   3.14000"
                                               // (width = 10, precision = 5)
auto s6 = std::format("{:{}.{}f}", pi, 10, 5); // s6 = "   3.14000"
                                               // (width = 10, precision = 5)

auto b1 = std::format("{:{}f}", pi, 10.0);     // throws: width is not of integral type
auto b2 = std::format("{:{}f}", pi, -10);      // throws: width is negative
auto b3 = std::format("{:.{}f}", pi, 5.0);     // throws: precision is not of integral type
```

The width of a string is defined as the estimated number of column positions
appropriate for displaying it in a terminal.

For the purpose of width computation, a string is assumed to be in an
implementation-defined encoding. The method of width computation is unspecified,
but for a string in a Unicode encoding, implementation should estimate the width
of the string as the sum of estimated widths of the first code points in its
extended grapheme clusters. The estimated width is 2 for the following code
points, and is 1 otherwise:

- Any code point whose Unicode property `East_Asian_Width` has value Fullwidth
  (`F`) or Wide (`W`)
- U+4DC0 - U+4DFF (Yijing Hexagram Symbols)
- U+1F300 – U+1F5FF (Miscellaneous Symbols and Pictographs)
- U+1F900 – U+1F9FF (Supplemental Symbols and Pictographs)

```cpp
auto s1 = std::format("{:.^5s}",   "🐱");      // s1 = ".🐱.."
auto s2 = std::format("{:.5s}",    "🐱🐱🐱");  // s2 = "🐱🐱"
auto s3 = std::format("{:.<5.5s}", "🐱🐱🐱");  // s3 = "🐱🐱."
```

##### L (locale-specific formatting)

The `L` option causes the locale-specific form to be used. This option is only
valid for arithmetic types.

- For integral types, the locale-specific form inserts the appropriate digit
  group separator characters according to the context's locale.
- For floating-point types, the locale-specific form inserts the appropriate
  digit group and radix separator characters according to the context's locale.
- For the textual representation of `bool`, the locale-specific form uses the
  appropriate string as if obtained with `std::numpunct::truename` or
  `std::numpunct::falsename`.

##### Type

The type option determines how the data should be presented.

The available string presentation types are:

- none, `s`: Copies the string to the output.

- `?`: Copies the escaped string (see below) to the output.
*(since C++23)*

The available integer presentation types for integral types other than char,
wchar_t, and bool are:

- `b`: Binary format. Produces the output as if by calling `std::to_chars(first,
  last, value, 2)`. The base prefix is `0b`.
- `B`: same as `b`, except that the base prefix is `0B`.
- `c`: Copies the character `static_cast<CharT>(value)` to the output, where
  `CharT` is the character type of the format string. Throws `std::format_error`
  if value is not in the range of representable values for `CharT`.
- `d`: Decimal format. Produces the output as if by calling
  `std::to_chars(first, last, value)`.
- `o`: Octal format. Produces the output as if by calling `std::to_chars(first,
  last, value, 8)`. The base prefix is `0` if the corresponding argument value
  is non-zero and is empty otherwise.
- `x`: Hex format. Produces the output as if by calling `std::to_chars(first,
  last, value, 16)`. The base prefix is `0x`.
- `X`: same as `x`, except that it uses uppercase letters for digits above 9 and
  the base prefix is `0X`.
- none: same as `d`.

The available char and wchar_t presentation types are:

- none, `c`: Copies the character to the output.
- `b`, `B`, `d`, `o`, `x`, `X`: Uses integer presentation types with the value
  `static_cast<unsigned char>(value)` or
  `static_cast<std::make_unsigned_t<wchar_t>>(value)` respectively.

- `?`: Copies the escaped character (see below) to the output.
*(since C++23)*

The available bool presentation types are:

- none, `s`: Copies textual representation (`true` or `false`, or the
  locale-specific form) to the output.
- `b`, `B`, `d`, `o`, `x`, `X`: Uses integer presentation types with the value
  `static_cast<unsigned char>(value)`.

The available floating-point presentation types are:

- `a`: If *precision* is specified, produces the output as if by calling
  `std::to_chars(first, last, value, std::chars_format::hex, precision)` where
  `precision` is the specified precision; otherwise, the output is produced as
  if by calling `std::to_chars(first, last, value, std::chars_format::hex)`.
- `A`: same as `a`, except that it uses uppercase letters for digits above 9 and
  uses `P` to indicate the exponent.
- `e`: Produces the output as if by calling `std::to_chars(first, last, value,
  std::chars_format::scientific, precision)` where `precision` is the specified
  precision, or 6 if precision is not specified.
- `E`: same as `e`, except that it uses `E` to indicate the exponent.
- `f`, `F`: Produces the output as if by calling `std::to_chars(first, last,
  value, std::chars_format::fixed, precision)` where `precision` is the
  specified precision, or 6 if precision is not specified.
- `g`: Produces the output as if by calling `std::to_chars(first, last, value,
  std::chars_format::general, precision)` where `precision` is the specified
  precision, or 6 if precision is not specified.
- `G`: same as `g`, except that it uses `E` to indicate the exponent.
- none: If *precision* is specified, produces the output as if by calling
  `std::to_chars(first, last, value, std::chars_format::general, precision)`
  where `precision` is the specified precision; otherwise, the output is
  produced as if by calling `std::to_chars(first, last, value)`.

For lower-case presentation types, infinity and NaN are formatted as `inf` and
`nan`, respectively. For upper-case presentation types, infinity and NaN are
formatted as `INF` and `NAN`, respectively.

The available pointer presentation types (also used for `std::nullptr_t`) are:

- none, `p`: If `std::uintptr_t` is defined, produces the output as if by
  calling `std::to_chars(first, last, reinterpret_cast<std::uintptr_t>(value),
  16)` with the prefix `0x` added to the output; otherwise, the output is
  implementation-defined.

- `P`: same as `p`, except that it uses uppercase letters for digits above 9 and
  the base prefix is `0X`.
*(since C++26)*

##### Formatting escaped characters and strings (since C++23)

A character or string can be formatted as *escaped* to make it more suitable for
debugging or for logging.

Escaping is done as follows:

- For each well-formed code unit sequence that encodes a character *C*:

  Character | Escape sequence | Notes
  horizontal tab (byte 0x09 in ASCII encoding) | `\t`
  line feed - new line (byte 0x0a in ASCII encoding) | `\n`
  carriage return (byte 0x0d in ASCII encoding) | `\r`
  double quote (byte 0x22 in ASCII encoding) | `\"` | Used only if the output is
      a double-quoted string
  single quote (byte 0x27 in ASCII encoding) | `\'` | Used only if the output is
      a single-quoted string
  backslash (byte 0x5c in ASCII encoding) | `\\`

- Otherwise, if *C* is not the space character (byte 0x20 in ASCII encoding),
  and either

  - the associated character encoding is a Unicode encoding and
  - the associated character encoding is not a Unicode encoding and *C* is one of
    an implementation-defined set of separator or non-printable characters

  the escape sequence is `\u{hex-digit-sequence}`, where `hex-digit-sequence` is
  the shortest hexadecimal representation of *C* using lower-case hexadecimal
  digits.

- Otherwise, *C* is copied as is.
- A code unit sequence that is a shift sequence has unspecified effect on the
  output and further decoding of the string.
- Other code units (i.e. those in ill-formed code unit sequences) are each
  replaced with `\x{hex-digit-sequence}`, where `hex-digit-sequence` is the
  shortest hexadecimal representation of the code unit using lower-case
  hexadecimal digits.

The escaped string representation of a string is constructed by escaping the
code unit sequences in the string, as described above, and quoting the result
with double quotes.

The escaped representation of a character is constructed by escaping it as
described above, and quoting the result with single quotes.

```cpp
auto s1 = std::format("[{:?}]", "h\tllo");             // s1 has value: ["h\tllo"]
auto s2 = std::format("[{:?}]", "Спасибо, Виктор ♥!"); // s2 has value:
                                                       //     ["Спасибо, Виктор ♥!"]
auto s3 = std::format("[{:?}] [{:?}]", '\'', '"');     // s3 has value: ['\'', '"']

// The following examples assume use of the UTF-8 encoding
auto s4 = std::format("[{:?}]", std::string("\0 \n \t \x02 \x1b", 9));
                                                  // s4 has value:
                                                  //     [\u{0} \n \t \u{2} \u{1b}]
auto s5 = std::format("[{:?}]", "\xc3\x28");      // invalid UTF-8
                                                  // s5 has value: ["\x{c3}("]
auto s6 = std::format("[{:?}]", "\u0301");        // s6 has value: ["\u{301}"]
auto s7 = std::format("[{:?}]", "\\\u0301");      // s7 has value: ["\\\u{301}"]
auto s8 = std::format("[{:?}]", "e\u0301\u0323"); // s8 has value: ["ẹ́"]
```

### Standard specializations for library types

- **std::formatter<std::chrono::duration> (C++20)** — formatting support for
  `duration` (class template specialization)
- **std::formatter<std::chrono::sys_time> (C++20)** — formatting support for
  `sys_time` (class template specialization)
- **std::formatter<std::chrono::utc_time> (C++20)** — formatting support for
  `utc_time` (class template specialization)
- **std::formatter<std::chrono::tai_time> (C++20)** — formatting support for
  `tai_time` (class template specialization)
- **std::formatter<std::chrono::gps_time> (C++20)** — formatting support for
  `gps_time` (class template specialization)
- **std::formatter<std::chrono::file_time> (C++20)** — formatting support for
  `file_time` (class template specialization)
- **std::formatter<std::chrono::local_time> (C++20)** — formatting support for
  `local_time` (class template specialization)
- **std::formatter<std::chrono::day> (C++20)** — formatting support for `day`
  (class template specialization)
- **std::formatter<std::chrono::month> (C++20)** — formatting support for
  `month` (class template specialization)
- **std::formatter<std::chrono::year> (C++20)** — formatting support for `year`
  (class template specialization)
- **std::formatter<std::chrono::weekday> (C++20)** — formatting support for
  `weekday` (class template specialization)
- **std::formatter<std::chrono::weekday_indexed> (C++20)** — formatting support
  for `weekday_indexed` (class template specialization)
- **std::formatter<std::chrono::weekday_last> (C++20)** — formatting support for
  `weekday_last` (class template specialization)
- **std::formatter<std::chrono::month_day> (C++20)** — formatting support for
  `month_day` (class template specialization)
- **std::formatter<std::chrono::month_day_last> (C++20)** — formatting support
  for `month_day_last` (class template specialization)
- **std::formatter<std::chrono::month_weekday> (C++20)** — formatting support
  for `month_weekday` (class template specialization)
- **std::formatter<std::chrono::month_weekday_last> (C++20)** — formatting
  support for `month_weekday_last` (class template specialization)
- **std::formatter<std::chrono::year_month> (C++20)** — formatting support for
  `year_month` (class template specialization)
- **std::formatter<std::chrono::year_month_day> (C++20)** — formatting support
  for `year_month_day` (class template specialization)
- **std::formatter<std::chrono::year_month_day_last> (C++20)** — formatting
  support for `year_month_day_last` (class template specialization)
- **std::formatter<std::chrono::year_month_weekday> (C++20)** — formatting
  support for `year_month_weekday` (class template specialization)
- **std::formatter<std::chrono::year_month_weekday_last> (C++20)** — formatting
  support for `year_month_weekday_last` (class template specialization)
- **std::formatter<std::chrono::hh_mm_ss> (C++20)** — formatting support for
  `hh_mm_ss` (class template specialization)
- **std::formatter<std::chrono::sys_info> (C++20)** — formatting support for
  `sys_info` (class template specialization)
- **std::formatter<std::chrono::local_info> (C++20)** — formatting support for
  `local_info` (class template specialization)
- **std::formatter<std::chrono::zoned_time> (C++20)** — formatting support for
  `zoned_time` (class template specialization)
- **std::formatter<std::basic_stacktrace> (C++23)** — formatting support for
  `basic_stacktrace` (class template specialization)
- **std::formatter<std::stacktrace_entry> (C++23)** — formatting support for
  `stacktrace_entry` (class template specialization)
- **std::formatter<std::thread::id> (C++23)** — formatting support for
  `thread::id` (class template specialization)
- **std::formatter<std::vector<bool>::reference> (C++23)** — formatting support
  for `vector<bool>::reference` (class template specialization)
- **std::formatter<*pair-or-tuple*> (C++23)** — formatting support for `pair`
  and `tuple` (class template specialization)
- **std::formatter<*range*> (C++23)** — formatting support for ranges (class
  template specialization)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_format_uchar` | 202311L | (C++20) (DR) | Formatting of code units
      as unsigned integers

### Example

```cpp
#include <algorithm>
#include <format>
#include <iomanip>
#include <iostream>
#include <sstream>
#include <string_view>

struct QuotableString : std::string_view
{};

template<>
struct std::formatter<QuotableString, char>
{
    bool quoted = false;

    template<class ParseContext>
    constexpr ParseContext::iterator parse(ParseContext& ctx)
    {
        auto it = ctx.begin();
        if (it == ctx.end())
            return it;

        if (*it == '#')
        {
            quoted = true;
            ++it;
        }
        if (*it != '}')
            throw std::format_error("Invalid format args for QuotableString.");

        return it;
    }

    template<class FmtContext>
    FmtContext::iterator format(QuotableString s, FmtContext& ctx) const
    {
        std::ostringstream out;
        if (quoted)
            out << std::quoted(s);
        else
            out << s;

        return std::ranges::copy(std::move(out).str(), ctx.out()).out;
    }
};

int main()
{
    QuotableString a("be"), a2(R"( " be " )");
    QuotableString b("a question");
    std::cout << std::format("To {0} or not to {0}, that is {1}.\n", a, b);
    std::cout << std::format("To {0:} or not to {0:}, that is {1:}.\n", a, b);
    std::cout << std::format("To {0:#} or not to {0:#}, that is {1:#}.\n", a2, b);
}
```

Output:

```text
To be or not to be, that is a question.
To be or not to be, that is a question.
To " \" be \" " or not to " \" be \" ", that is "a question".
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2909R4 | C++20 | char or wchar_t might be formatted as out-of-range unsigned
      integer values | code units are converted to the corresponding unsigned
      type before such formatting
  LWG 3721 | C++20 | zero is not allowed for the width field in standard format
      specification | zero is permitted if specified via a replacement field

### See also

- **basic_format_contextformat_contextwformat_context (C++20)(C++20)(C++20)** —
  formatting state, including all formatting arguments and the output iterator
  (class template)
- **formattable (C++23)** — specifies that a type is formattable, that is, it
  specializes `std::formatter` and provides member functions `parse` and
  `format` (concept)

---
*Source: https://en.cppreference.com/w/cpp/utility/format/formatter*
