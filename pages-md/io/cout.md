# std::cout, std::wcout

`std::cout` is the global `std::ostream` wired to the process's standard
output (`stdout`); `std::cout << x` is how most C++ programs print. It's
guaranteed initialized before any code that runs after `<iostream>` is
included can observe it, and by default it's synchronized both with C's
`stdout` and across threads.

```cpp skip
extern std::ostream cout;   // writes to stdout
extern std::wostream wcout; // wide version, writes to stdout
```

### Guarantees and costs

- `std::cout`/`std::wcout` are guaranteed initialized during or before the
  first construction of a `std::ios_base::Init` object — in practice, as
  long as `<iostream>` is included before your static objects run, `cout`
  is safe to use from their constructors and destructors.
- By default (`std::ios_base::sync_with_stdio` still `true`), it's safe to
  use `cout` concurrently from multiple threads for both formatted and
  unformatted output, and `cout`/`printf` interleave correctly. Calling
  `sync_with_stdio(false)` drops that guarantee in exchange for speed.
- `std::cin.tie()` returns `&std::cout`, so any input operation on `cin`
  first calls `cout.flush()` — interactive prompt-then-read code doesn't
  need an explicit flush before reading. `std::cerr.tie()` returns
  `&std::cout` too (since C++11), so writing to `cerr` also flushes `cout`
  first, keeping interleaved output and error messages in order.

### Gotchas

- Templated variables with dynamic initialization run in unspecified
  order relative to `cout`'s own initialization unless a
  `std::ios_base::Init` object has already been constructed — don't assume
  `cout` is ready inside such a variable's initializer without one.
- `sync_with_stdio(false)` is a common performance tweak, but it forfeits
  both the C-stdio interleaving guarantee and the multithread safety
  guarantee above — apply it only when the program doesn't rely on either.

### Example

```cpp
#include <iostream>

struct Foo
{
    Foo() { std::cout << "static constructor\n"; }
    ~Foo() { std::cout << "static destructor\n"; }
};

Foo f;  // static object; <iostream> above guarantees cout is ready for it

int main()
{
    std::cout << "main function\n";
}
```

```text
static constructor
main function
static destructor
```

### See also

- **cerr** — unbuffered, tied to `cout`, for immediate error output
- **basic_ostream** — the type `cout` is an instance of
- **Init** — the object whose construction guarantees `cout` is ready

---
*Source: https://en.cppreference.com/w/cpp/io/cout*
