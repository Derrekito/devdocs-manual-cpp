# std::ios_base::sync_with_stdio

```cpp
static bool sync_with_stdio( bool sync = true );
```

Sets whether the standard C++ streams are synchronized to the standard C streams
after each input/output operation.

The standard C++ streams are the following: `std::cin`, `std::cout`,
`std::cerr`, `std::clog`, `std::wcin`, `std::wcout`, `std::wcerr` and
`std::wclog`.

The standard C streams are the following: `stdin`, `stdout` and `stderr`.

For a standard stream `str`, synchronized with the C stream `f`, the following
pairs of functions have identical effect:

1) `std::fputc(f, c)` and `str.rdbuf()->sputc(c)`.

2) `std::fgetc(f)` and `str.rdbuf()->sbumpc()`.

3) `std::ungetc(c, f)` and `str.rdbuf()->sputbackc(c)`.

In practice, this means that the synchronized C++ streams are unbuffered, and
each I/O operation on a C++ stream is immediately applied to the corresponding C
stream's buffer. This makes it possible to freely mix C++ and C I/O.

In addition, synchronized C++ streams are guaranteed to be thread-safe
(individual characters output from multiple threads may interleave, but no data
races occur).

If the synchronization is turned off, the C++ standard streams are allowed to
buffer their I/O independently, which may be considerably faster in some cases.

By default, all eight standard C++ streams are synchronized with their
respective C streams.

If this function is called after I/O has occurred on the standard stream, the
behavior is implementation-defined: implementations range from no effect to
destroying the read buffer.

### Parameters

- **sync** — the new synchronization setting

### Return value

Synchronization state before the call to the function.

### Example

```cpp
#include <cstdio>
#include <iostream>

int main()
{
    std::ios::sync_with_stdio(false);
    std::cout << "a\n";
    std::printf("b\n");
    std::cout << "c\n";
}
```

Possible output:

```text
b
a
c
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 49 | C++98 | it was unspecified (1) which state is actually returned and
      (2) what does 'synchronized' between standard C and C++ streams mean |
      both specified

### See also

- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **cerrwcerr** — writes to the standard C error stream `stderr`, unbuffered
  (global object)
- **clogwclog** — writes to the standard C error stream `stderr` (global object)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/sync_with_stdio*
