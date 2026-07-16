# Filename and line information

Changes the source code's line number and, optionally, the current file name, in
the preprocessor.

### Syntax

```text
#line lineno    (1)
#line lineno "filename"    (2)
```

### Explanation

1) Changes the current preprocessor line number to lineno. Expansions of the
macro `__LINE__` beyond this point will expand to lineno plus the number of
actual source code lines encountered since.

2) Also changes the current preprocessor file name to filename. Expansions of
the macro `__FILE__` from this point will produce filename.

Any preprocessing tokens (macro constants or expressions) are permitted as
arguments to `#line` as long as they expand to a valid decimal integer
optionally following a valid character string.

lineno must be a sequence of at least one decimal digit (the program is
ill-formed, otherwise) and is always interpreted as decimal (even if it starts
with `0`).

If lineno is `0` or greater than `32767`(until C++11)`2147483647`(since C++11),
the behavior is undefined.

### Notes

This directive is used by some automatic code generation tools which produce C++
source files from a file written in another language. In that case, `#line`
directives may be inserted in the generated C++ file referencing line numbers
and the file name of the original (human-editable) source file.

### Example

```cpp
#include <cassert>
#define FNAME "test.cc"
int main()
{
#line 777 FNAME
        assert(2+2 == 5);
}
```

Possible output:

```text
test: test.cc:777: int main(): Assertion `2+2 == 5' failed.
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++98 standard (ISO/IEC 14882:1998):

### See also

- **source_location (C++20)** — a class representing information about the
  source code, such as file names, line numbers, and function names (class)

**C documentation for Filename and line information**

---
*Source: https://en.cppreference.com/w/cpp/preprocessor/line*
