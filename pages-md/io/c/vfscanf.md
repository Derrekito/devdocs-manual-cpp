# std::vscanf, std::vfscanf, std::vsscanf

```cpp
int vscanf( const char* format, std::va_list vlist );  // (1) (since C++11)
int vfscanf( std::FILE* stream, const char* format, std::va_list vlist );  // (2) (since C++11)
int vsscanf( const char* buffer, const char* format, std::va_list vlist );  // (3) (since C++11)
```

Reads data from a variety of sources, interprets it according to `format` and
stores the results into locations defined by `vlist`.

1) Reads the data from `stdin`.

2) Reads the data from file stream `stream`.

3) Reads the data from null-terminated character string `buffer`.

### Parameters

- **stream** — input file stream to read from
- **buffer** — pointer to a null-terminated character string to read from
- **format** — pointer to a null-terminated character string specifying how to
  read the input
- **vlist** — variable argument list containing the receiving arguments.

The **format** string consists of

- non-whitespace multibyte characters except `%`: each such character in the
  format string consumes exactly one identical character from the input stream,
  or causes the function to fail if the next character on the stream does not
  compare equal.
- whitespace characters: any single whitespace character in the format string
  consumes all available consecutive whitespace characters from the input
  (determined as if by calling `isspace` in a loop). Note that there is no
  difference between `"\n"`, `" "`, `"\t\t"`, or other whitespace in the format
  string.
- conversion specifications. Each conversion specification has the following
  format:

The following format specifiers are available:

  Conversion specifier | Explanation | Argument type
  **Length modifier →** | `hh` (C++11) | `h` | (none) | `l` | `ll` (C++11) | `j`
      (C++11) | `z` (C++11) | `t` (C++11) | `L`
  `%` | Matches literal `%`. | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
      N/A
  `c` | Matches a **character** or a sequence of **characters**. If a width
      specifier is used, matches exactly *width* characters (the argument must
      be a pointer to an array with sufficient room). Unlike %s and %[, does not
      append the null character to the array. | N/A | N/A | `char*` | `wchar_t*`
      | N/A | N/A | N/A | N/A | N/A
  `s` | Matches a sequence of non-whitespace characters (a **string**). If width
      specifier is used, matches up to *width* or until the first whitespace
      character, whichever appears first. Always stores a null character in
      addition to the characters matched (so the argument array must have room
      for at least *width+1* characters)
  `[`set`]` | Matches a non-empty sequence of character from set of characters.
      If the first character of the set is `^`, then all characters not in the
      set are matched. If the set begins with `]` or `^]` then the `]` character
      is also included into the set. It is implementation-defined whether the
      character `-` in the non-initial position in the scanset may be indicating
      a range, as in `[0-9]`. If width specifier is used, matches only up to
      *width*. Always stores a null character in addition to the characters
      matched (so the argument array must have room for at least *width+1*
      characters)
  `d` | Matches a **decimal integer**. The format of the number is the same as
      expected by `strtol` with the value `10` for the `base` argument | `signed
      char*` or `unsigned char*` | `signed short*` or `unsigned short*` |
      `signed int*` or `unsigned int*` | `signed long*` or `unsigned long*` |
      `signed long long*` or `unsigned long long*` | `intmax_t*` or `uintmax_t*`
      | `size_t*` | `ptrdiff_t*` | N/A
  `i` | Matches an **integer**. The format of the number is the same as expected
      by `strtol` with the value `​0​` for the `base` argument (base is
      determined by the first characters parsed)
  `u` | Matches an unsigned **decimal integer**. The format of the number is the
      same as expected by `strtoul` with the value `10` for the `base` argument.
  `o` | Matches an unsigned **octal integer**. The format of the number is the
      same as expected by `strtoul` with the value `8` for the `base` argument
  `x`, `X` | Matches an unsigned **hexadecimal integer**. The format of the
      number is the same as expected by `strtoul` with the value `16` for the
      `base` argument
  `n` | Returns the **number of characters read so far**. No input is consumed.
      Does not increment the assignment count. If the specifier has
      assignment-suppressing operator defined, the behavior is undefined
  `a`, `A`(C++11) `e`, `E` `f`, `F` `g`, `G` | Matches a **floating-point
      number**. The format of the number is the same as expected by `strtof` |
      N/A | N/A | `float*` | `double*` | N/A | N/A | N/A | N/A | `long double*`
  `p` | Matches implementation defined character sequence defining a
      **pointer**. `printf` family of functions should produce the same sequence
      using `%p` format specifier | N/A | N/A | `void**` | N/A | N/A | N/A | N/A
      | N/A | N/A

For every conversion specifier other than `n`, the longest sequence of input
characters which does not exceed any specified ﬁeld width and which either is
exactly what the conversion specifier expects or is a prefix of a sequence it
would expect, is what's consumed from the stream. The ﬁrst character, if any,
after this consumed sequence remains unread. If the consumed sequence has length
zero or if the consumed sequence cannot be converted as specified above, the
matching failure occurs unless end-of-ﬁle, an encoding error, or a read error
prevented input from the stream, in which case it is an input failure.

All conversion specifiers other than `[`, `c`, and `n` consume and discard all
leading whitespace characters (determined as if by calling `isspace`) before
attempting to parse the input. These consumed characters do not count towards
the specified maximum field width.

The conversion specifiers `lc`, `ls`, and `l[` perform multibyte-to-wide
character conversion as if by calling `mbrtowc` with an `mbstate_t` object
initialized to zero before the first character is converted.

The conversion specifiers `s` and `[` always store the null terminator in
addition to the matched characters. The size of the destination array must be at
least one greater than the specified field width. The use of `%s` or `%[`,
without specifying the destination array size, is as unsafe as `std::gets`.

The correct conversion specifications for the fixed-width integer types
(`int8_t`, etc) are defined in the header `<cinttypes>` (although `SCNdMAX`,
`SCNuMAX`, etc is synonymous with `%jd`, `%ju`, etc).

There is a sequence point after the action of each conversion specifier; this
permits storing multiple fields in the same "sink" variable.

When parsing an incomplete floating-point value that ends in the exponent with
no digits, such as parsing `"100er"` with the conversion specifier `%f`, the
sequence `"100e"` (the longest prefix of a possibly valid floating-point number)
is consumed, resulting in a matching error (the consumed sequence cannot be
converted to a floating-point number), with `"r"` remaining. Some existing
implementations do not follow this rule and roll back to consume only `"100"`,
leaving `"er"`, e.g. glibc bug 1765.

### Return value

Number of arguments successfully read, or `EOF` if failure occurs.

### Notes

All these functions invoke `va_arg` at least once, the value of `arg` is
indeterminate after the return. These functions to not invoke `va_end`, and it
must be done by the caller.

### Example

```cpp
#include <cstdarg>
#include <cstdio>
#include <iostream>
#include <stdexcept>

void checked_sscanf(int count, const char* buf, const char *fmt, ...)
{
    std::va_list ap;
    va_start(ap, fmt);
    if (std::vsscanf(buf, fmt, ap) != count)
        throw std::runtime_error("parsing error");
    va_end(ap);
}

int main()
{
    try
    {
        int n, m;
        std::cout << "Parsing '1 2'... ";
        checked_sscanf(2, "1 2", "%d %d", &n, &m);
        std::cout << "success\n";
        std::cout << "Parsing '1 a'... ";
        checked_sscanf(2, "1 a", "%d %d", &n, &m);
        std::cout << "success\n";
    }
    catch (const std::exception& e)
    {
        std::cout << e.what() << '\n';
    }
}
```

Output:

```text
Parsing '1 2'... success
Parsing '1 a'... parsing error
```

### See also

- **scanffscanfsscanf** — reads formatted input from `stdin`, a file stream or a
  buffer (function)
- **vprintfvfprintfvsprintfvsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer using variable argument list (function)

**C documentation for `vscanf, vfscanf, vsscanf`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/vfscanf*
