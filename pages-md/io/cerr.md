# std::cerr, std::wcerr

```cpp
extern std::ostream cerr;  // (1)
extern std::wostream wcerr;  // (2)
```

The global objects `std::cerr` and `std::wcerr` control output to a stream
buffer of implementation-defined type (derived from `std::streambuf` and
`std::wstreambuf`, respectively), associated with the standard C error output
stream `stderr`.

These objects are guaranteed to be initialized during or before the first time
an object of type `std::ios_base::Init` is constructed and are available for use
in the constructors and destructors of static objects with ordered
initialization (as long as `<iostream>` is included before the object is
defined).

Unless `std::ios_base::sync_with_stdio(false)` has been issued, it is safe to
concurrently access these objects from multiple threads for both formatted and
unformatted output.

Once initialized, `(std::cerr.flags() & unitbuf) != 0` (same for `std::wcerr`)
meaning that any output sent to these stream objects is immediately flushed to
the OS (via `std::basic_ostream::sentry`'s destructor).

In addition, `std::cerr.tie()` returns `&std::cout` (same for `std::wcerr` and
`std::wcout`), meaning that any output operation on `std::cerr` first executes
`std::cout.flush()` (via `std::basic_ostream::sentry`'s constructor).

### Notes

The 'c' in the name refers to "character" (stroustrup.com FAQ); `cerr` means
"character error (stream)" and `wcerr` means "wide character error (stream)".

### Example

Output to `stderr` via `std::cerr` flushes out the pending output on
`std::cout`, while output to `stderr` via `std::clog` does not.

```cpp
#include <chrono>
#include <iostream>
#include <thread>
using namespace std::chrono_literals;

void f()
{
    std::cout << "Output from thread...";
    std::this_thread::sleep_for(2s);
    std::cout << "...thread calls flush()" << std::endl;
}

int main()
{
    std::jthread t1{f};
    std::this_thread::sleep_for(1000ms);
    std::clog << "This output from main is not tie()'d to cout\n";
    std::cerr << "This output is tie()'d to cout\n";
}
```

Possible output:

```text
This output from main is not tie()'d to cout
Output from thread...This output is tie()'d to cout
...thread calls flush()
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 455 | C++98 | `std::cerr.tie()` and `std::wcerr.tie()` returned null
      pointers | they return `&std::cout` and `&std::wcout` respectively

### See also

- **Init** — initializes standard stream objects (public member class of
  `std::ios_base`)
- **clogwclog** — writes to the standard C error stream `stderr` (global object)
- **coutwcout** — writes to the standard C output stream `stdout` (global
  object)
- **stdinstdoutstderr** — expression of type `FILE*` associated with the input
  stream expression of type `FILE*` associated with the output stream expression
  of type `FILE*` associated with the error output stream (macro constant)

---
*Source: https://en.cppreference.com/w/cpp/io/cerr*
