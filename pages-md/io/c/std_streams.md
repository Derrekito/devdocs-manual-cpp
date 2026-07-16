# stdin, stdout, stderr

```cpp
#define stdin  /* implementation-defined */  // (1)
#define stdout /* implementation-defined */  // (2)
#define stderr /* implementation-defined */  // (3)
```

Three text streams are predefined. These streams are implicitly opened and
unoriented at program startup.

1) Associated with the *standard input* stream, used for reading conventional
   input. At program startup, the stream is fully buffered if and only if the
   stream can be determined not to refer to an interactive device.

2) Associated with the *standard output* stream, used for writing conventional
   output. At program startup, the stream is fully buffered if and only if the
   stream can be determined not to refer to an interactive device.

3) Associated with the *standard error* stream, used for writing diagnostic
   output. At program startup, the stream is not fully buffered.

What constitutes an interactive device is implementation-defined.

These macros are expanded to expressions of type `std::FILE*`.

### Notes

Although not mandated by POSIX, the UNIX convention is that `stdin` and `stdout`
are line-buffered if associated with a terminal and `stderr` is unbuffered.

These macros may be expanded to modifiable lvalues. If any of these `std::FILE*`
lvalue is modified, subsequent operations on the corresponding stream result in
unspecified or undefined behavior.

### Example

This example shows a function similar to `std::printf`.

```cpp
#include <concepts>
#include <cstdio>
#include <type_traits>

template<typename T>
concept IsPrintable = std::integral<T> or std::floating_point<T> or std::is_pointer_v<T>;

int my_printf(char const* const format, IsPrintable auto const ... arguments)
{
    return std::fprintf(stdout, format, arguments...);
}

int main(int argv, char*[])
{
    my_printf("Strings and chars:\t%s %c\n", "hello", 'x');
    my_printf("Rounding:\t\t%f %.0f %.32f\n", 1.5, 1.5, 1.3);
    my_printf("Padding:\t\t%05.2f %.2f %5.2f\n", 1.5, 1.5, 1.5);
    my_printf("Scientific:\t\t%E %e\n", 1.5, 1.5);
    my_printf("Hexadecimal:\t\t%a %A 0x%X\n", 1.5, 1.5, &argv);
}
```

Possible output:

```text
Strings and chars:  hello x
Rounding:           1.500000 2 1.30000000000000004440892098500626
Padding:            01.50 1.50  1.50
Scientific:         1.500000E+00 1.500000e+00
Hexadecimal:        0x1.8p+0 0X1.8P+0 0x2CFB41BC
```

### See also

- **cinwcin** — reads from the standard C input stream `stdin` (global object)
- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **cerrwcerr** — writes to the standard C error stream `stderr`, unbuffered
  (global object)
- **clogwclog** — writes to the standard C error stream `stderr` (global object)
- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)
- **FILE** — object type, capable of holding all information needed to control a
  C I/O stream (typedef)

**C documentation for `stdin, stdout, stderr`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/std_streams*
