# std::cin, std::wcin

```cpp
extern std::istream cin;  // (1)
extern std::wistream wcin;  // (2)
```

The global objects `std::cin` and `std::wcin` control input from a stream buffer
of implementation-defined type (derived from `std::streambuf`), associated with
the standard C input stream `stdin`.

These objects are guaranteed to be initialized during or before the first time
an object of type `std::ios_base::Init` is constructed and are available for use
in the constructors and destructors of static objects with ordered
initialization (as long as `<iostream>` is included before the object is
defined).

Unless `sync_with_stdio(false)` has been issued, it is safe to concurrently
access these objects from multiple threads for both formatted and unformatted
input.

Once `std::cin` is constructed, `std::cin.tie()` returns `&std::cout`, and
likewise, `std::wcin.tie()` returns `&std::wcout`. This means that any formatted
input operation on `std::cin` forces a call to `std::cout.flush()` if any
characters are pending for output.

### Notes

The 'c' in the name refers to "character" (stroustrup.com FAQ); `cin` means
"character input" and `wcin` means "wide character input".

### Example

```cpp
#include <iostream>

struct Foo
{
    int n;
    Foo()
    {
        std::cout << "Enter n: "; // no flush needed
        std::cin >> n;
    }
};

Foo f; // static object

int main()
{
    std::cout << "f.n is " << f.n << '\n';
}
```

Possible output:

```text
Enter n: 10
f.n is 10
```

### See also

- **Init** — initializes standard stream objects (public member class of
  `std::ios_base`)
- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **stdinstdoutstderr** — expression of type `FILE*` associated with the input
  stream expression of type `FILE*` associated with the output stream expression
  of type `FILE*` associated with the error output stream (macro constant)

---
*Source: https://en.cppreference.com/w/cpp/io/cin*
