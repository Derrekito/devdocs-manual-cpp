# std::vprintf, std::vfprintf, std::vsprintf, std::vsnprintf

```cpp
int vprintf( const char* format, std::va_list vlist );  // (1)
int vfprintf( std::FILE* stream, const char* format, std::va_list vlist );  // (2)
int vsprintf( char* buffer, const char* format, std::va_list vlist );  // (3)
int vsnprintf( char* buffer, std::size_t buf_size, const char* format, std::va_list vlist );  // (4) (since C++11)
```

Loads the data from the locations, defined by `vlist`, converts them to
character string equivalents and writes the results to a variety of sinks.

1) Writes the results to `stdout`.

2) Writes the results to a file stream `stream`.

3) Writes the results to a character string `buffer`.

4) Writes the results to a character string `buffer`. At most `buf_size - 1`
   characters are written. The resulting character string will be terminated
   with a null character, unless `buf_size` is zero. If `buf_size` is zero,
   nothing is written and `buffer` may be a null pointer, however the return
   value (number of bytes that would be written not including the null
   terminator) is still calculated and returned.

### Parameters

- **stream** — output file stream to write to
- **buffer** — pointer to a character string to write to
- **buf_size** — maximum number of characters to write
- **format** — pointer to a null-terminated character string specifying how to
  interpret the data
- **vlist** — variable argument list containing the data to print

The **format** string consists of ordinary byte characters (except `%`), which
are copied unchanged into the output stream, and conversion specifications. Each
conversion specification has the following format:

- introductory `%` character.
- (optional) one or more flags that modify the behavior of the conversion:
- (optional) integer value or `*` that specifies minimum field width. The result
  is padded with *space* characters (by default), if required, on the left when
  right-justified, or on the right if left-justified. In the case when `*` is
  used, the width is specified by an additional argument of type `int`, which
  appears before the argument to be converted and the argument supplying
  precision if one is supplied. If the value of the argument is negative, it
  results with the `-` flag specified and positive field width (Note: This is
  the minimum width: The value is never truncated.).

The following format specifiers are available:

  Conversion Specifier | Explanation | Expected Argument Type
  **Length Modifier****→** | `hh` (C++11) | `h` | (none) | `l` | `ll` (C++11) |
      `j` (C++11) | `z` (C++11) | `t` (C++11) | `L`
  `%` | Writes literal `%`. The full conversion specification must be `%%`. |
      N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A
  `c` | Writes a **single character**. The argument is first converted to
      `unsigned char`. If the **l** modifier is used, the argument is first
      converted to a character string as if by **%ls** with a `wchar_t[2]`
      argument. | N/A | N/A | `int` | `wint_t` | N/A | N/A | N/A | N/A | N/A
  `s` | Writes a **character string** The argument must be a pointer to the
      initial element of an array of characters. *Precision* specifies the
      maximum number of bytes to be written. If *Precision* is not specified,
      writes every byte up to and not including the first null terminator. If
      the **l** specifier is used, the argument must be a pointer to the initial
      element of an array of `wchar_t`, which is converted to char array as if
      by a call to `wcrtomb` with zero-initialized conversion state. | N/A | N/A
      | `char*` | `wchar_t*` | N/A | N/A | N/A | N/A | N/A
  `d` `i` | Converts a **signed integer** into decimal representation *[-]dddd*.
      *Precision* specifies the minimum number of digits to appear. The default
      precision is `1`. If both the converted value and the precision are `​0​`
      the conversion results in no characters. | `signed char` | `short` | `int`
      | `long` | `long long` | `intmax_t` | signed `size_t` | `ptrdiff_t` | N/A
  `o` | Converts an **unsigned integer** into octal representation *oooo*.
      *Precision* specifies the minimum number of digits to appear. The default
      precision is `1`. If both the converted value and the precision are `​0​`
      the conversion results in no characters. In the *alternative
      implementation* precision is increased if necessary, to write one leading
      zero. In that case if both the converted value and the precision are
      `​0​`, single `​0​` is written. | `unsigned char` | `unsigned short` |
      `unsigned int` | `unsigned long` | `unsigned long long` | `uintmax_t` |
      `size_t` | unsigned version of `ptrdiff_t` | N/A
  `x` `X` | Converts an **unsigned integer** into hexadecimal representation
      *hhhh*. For the `x` conversion letters `abcdef` are used. For the `X`
      conversion letters `ABCDEF` are used. *Precision* specifies the minimum
      number of digits to appear. The default precision is `1`. If both the
      converted value and the precision are `​0​` the conversion results in no
      characters. In the *alternative implementation* `0x` or `0X` is prefixed
      to results if the converted value is nonzero. | N/A
  `u` | Converts an **unsigned integer** into decimal representation *dddd*.
      *Precision* specifies the minimum number of digits to appear. The default
      precision is `1`. If both the converted value and the precision are `​0​`
      the conversion results in no characters. | N/A
  `f` `F` | Converts **floating-point number** to the decimal notation in the
      style *[-]ddd.ddd*. *Precision* specifies the exact number of digits to
      appear after the decimal point character. The default precision is `6`. In
      the *alternative implementation* decimal point character is written even
      if no digits follow it. For infinity and not-a-number conversion style see
      notes. | N/A | N/A | `double` | `double`(C++11) | N/A | N/A | N/A | N/A |
      `long double`
  `e` `E` | Converts **floating-point number** to the decimal exponent notation.
      For the `e` conversion style *[-]d.ddd*`e`*±dd* is used. For the `E`
      conversion style *[-]d.ddd*`E`*±dd* is used. The exponent contains at
      least two digits, more digits are used only if necessary. If the value is
      `​0​`, the exponent is also `​0​`. *Precision* specifies the exact number
      of digits to appear after the decimal point character. The default
      precision is `6`. In the *alternative implementation* decimal point
      character is written even if no digits follow it. For infinity and
      not-a-number conversion style see notes. | N/A | N/A | N/A | N/A | N/A |
      N/A
  `a` `A` (C++11) | Converts **floating-point number** to the hexadecimal
      exponent notation. For the `a` conversion style *[-]*`0x`*h.hhh*`p`*±d* is
      used. For the `A` conversion style *[-]*`0X`*h.hhh*`P`*±d* is used. The
      first hexadecimal digit is not `0` if the argument is a normalized
      floating point value. If the value is `​0​`, the exponent is also `​0​`.
      *Precision* specifies the exact number of digits to appear after the
      hexadecimal point character. The default precision is sufficient for exact
      representation of the value. In the *alternative implementation* decimal
      point character is written even if no digits follow it. For infinity and
      not-a-number conversion style see notes. | N/A | N/A | N/A | N/A | N/A |
      N/A
  `g` `G` | Converts **floating-point number** to decimal or decimal exponent
      notation depending on the value and the *precision*. For the `g`
      conversion style conversion with style `e` or `f` will be performed. For
      the `G` conversion style conversion with style `E` or `F` will be
      performed. Let `P` equal the precision if nonzero, `6` if the precision is
      not specified, or `1` if the precision is `​0​`. Then, if a conversion
      with style `E` would have an exponent of `X`: if *P > X ≥ −4*, the
      conversion is with style `f` or `F` and precision *P − 1 − X*. otherwise,
      the conversion is with style `e` or `E` and precision *P − 1*. Unless
      *alternative representation* is requested the trailing zeros are removed,
      also the decimal point character is removed if no fractional part is left.
      For infinity and not-a-number conversion style see notes. | N/A | N/A |
      N/A | N/A | N/A | N/A
  `n` | Returns the **number of characters written** so far by this call to the
      function. The result is *written* to the value pointed to by the argument.
      The specification may not contain any *flag*, *field width*, or
      *precision*. | `signed char*` | `short*` | `int*` | `long*` | `long long*`
      | `intmax_t*` | signed `size_t*` | `ptrdiff_t*` | N/A
  `p` | Writes an implementation defined character sequence defining a
      **pointer**. | N/A | N/A | `void*` | N/A | N/A | N/A | N/A | N/A | N/A

The floating point conversion functions convert infinity to `inf` or `infinity`.
Which one is used is implementation defined.

Not-a-number is converted to `nan` or `nan(char_sequence)`. Which one is used is
implementation defined.

The conversions `F`, `E`, `G`, `A` output `INF`, `INFINITY`, `NAN` instead.

Even though `%c` expects `int` argument, it is safe to pass a `char` because of
the integer promotion that takes place when a variadic function is called.

The correct conversion specifications for the fixed-width character types
(`int8_t`, etc) are defined in the header `<cinttypes>` (although `PRIdMAX`,
`PRIuMAX`, etc is synonymous with `%jd`, `%ju`, etc).

The memory-writing conversion specifier `%n` is a common target of security
exploits where format strings depend on user input and is not supported by the
bounds-checked `printf_s` family of functions.

There is a sequence point after the action of each conversion specifier; this
permits storing multiple `%n` results in the same variable or, as an edge case,
printing a string modified by an earlier `%n` within the same call.

If a conversion specification is invalid, the behavior is undefined.

### Return value

1-3) Number of characters written if successful or negative value if an error
   occurred.

4) Number of characters written if successful or negative value if an error
   occurred. If the resulting string gets truncated due to `buf_size` limit,
   function returns the total number of characters (not including the
   terminating null-byte) which would have been written, if the limit was not
   imposed.

### Notes

All these functions invoke `va_arg` at least once, the value of `arg` is
indeterminate after the return. These functions do not invoke `va_end`, and it
must be done by the caller.

### Example

```cpp
#include <cstdarg>
#include <cstdio>
#include <ctime>
#include <vector>

void debug_log(const char *fmt, ...)
{
    std::time_t t = std::time(nullptr);
    char time_buf[100];
    std::strftime(time_buf, sizeof time_buf, "%D %T", std::gmtime(&t));
    std::va_list args1;
    va_start(args1, fmt);
    std::va_list args2;
    va_copy(args2, args1);
    std::vector<char> buf(1 + std::vsnprintf(nullptr, 0, fmt, args1));
    va_end(args1);
    std::vsnprintf(buf.data(), buf.size(), fmt, args2);
    va_end(args2);
    std::printf("%s [debug]: %s\n", time_buf, buf.data());
}

int main()
{
    debug_log("Logging, %d, %d, %d", 1, 2, 3);
}
```

Output:

```text
04/13/15 15:09:18 [debug]: Logging, 1, 2, 3
```

### See also

- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)
- **vscanfvfscanfvsscanf (C++11)(C++11)(C++11)** — reads formatted input from
  `stdin`, a file stream or a buffer using variable argument list (function)
- **vprint_unicode (C++23)** — prints to Unicode capable `stdout` or a file
  stream using type-erased argument representation (function)
- **vprint_nonunicode (C++23)** — prints to `stdout` or a file stream using
  type-erased argument representation (function)

**C documentation for `vprintf, vfprintf, vsprintf, vsnprintf`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/vfprintf*
