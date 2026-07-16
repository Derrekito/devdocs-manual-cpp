# std::clog, std::wclog

```cpp
extern std::ostream clog;  // (1)
extern std::wostream wclog;  // (2)
```

The global objects `std::clog` and `std::wclog` control output to a stream
buffer of implementation-defined type (derived from `std::streambuf`),
associated with the standard C output stream `stderr`, but, unlike
`std::cerr`/`std::wcerr`, these streams are not automatically flushed and cout
is not automatically tie()'d with these streams.

These objects are guaranteed to be initialized during or before the first time
an object of type `std::ios_base::Init` is constructed and are available for use
in the constructors and destructors of static objects with ordered
initialization (as long as `<iostream>` is included before the object is
defined).

Unless `std::ios_base::sync_with_stdio(false)` has been issued, it is safe to
concurrently access these objects from multiple threads for both formatted and
unformatted output.

### Notes

The 'c' in the name refers to "character" (stroustrup.com FAQ); `clog` means
"character log" and `wclog` means "wide character log".

### Example

```cpp
#include <iostream>

struct Foo
{
    int n;
    Foo()
    {
        std::clog << "static constructor\n";
    }
    ~Foo()
    {
        std::clog << "static destructor\n";
    }
};

Foo f; // static object

int main()
{
    std::clog << "main function\n";
}
```

Output:

```text
static constructor
main function
static destructor
```

### See also

- **Init** — initializes standard stream objects (public member class of
  `std::ios_base`)
- **cerrwcerr** — writes to the standard C error stream `stderr`, unbuffered
  (global object)
- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **stdinstdoutstderr** — expression of type `FILE*` associated with the input
  stream expression of type `FILE*` associated with the output stream expression
  of type `FILE*` associated with the error output stream (macro constant)

---
*Source: https://en.cppreference.com/w/cpp/io/clog*
