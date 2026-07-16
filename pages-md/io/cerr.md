# std::cerr, std::wcerr

`std::cerr` is the global `std::ostream` wired to the process's standard
error (`stderr`). It's **unbuffered** — every write reaches the OS
immediately — and **tied** to `std::cout`, so writing to `cerr` first
flushes any output still pending on `cout`. That's why diagnostics and
error messages are written to `cerr` rather than `cout`: they show up right
away, in the correct order relative to normal output, even if the program
crashes moments later.

```cpp skip
extern std::ostream cerr;   // writes to stderr, unbuffered
extern std::wostream wcerr; // wide version, writes to stderr, unbuffered
```

### Guarantees and costs

- `std::cerr`/`std::wcerr` are guaranteed initialized during or before the
  first construction of a `std::ios_base::Init` object, so they're usable
  from the constructors/destructors of static objects (as long as
  `<iostream>` is included first).
- Once initialized, `cerr` always has `std::ios_base::unitbuf` set: output
  is flushed to the OS after every insertion, unlike the general buffering
  `cout` does. This costs throughput — don't reach for `cerr` for high
  volume output, only for diagnostics.
- `cerr.tie()` returns `&std::cout` (same for `wcerr`/`wcout`), so any
  output operation on `cerr` first executes `cout.flush()` — interleaved
  `cout`/`cerr` output stays in the order you wrote it.
- Unless `sync_with_stdio(false)` has been issued, concurrent use of `cerr`
  from multiple threads is safe for both formatted and unformatted output.

### Gotchas

- Because `cerr` is unbuffered and flushes `cout` on every write, mixing
  heavy `cerr` logging into a hot path is slow — reach for `clog` (buffered,
  not tied to `cout`) instead if you want stderr output without the
  per-write flush cost.
- Reading `cerr`'s output right requires reading it separately from
  `cout`'s: they're different OS streams (`stderr` vs `stdout`), so
  redirecting one doesn't capture the other unless you merge them
  explicitly (e.g. shell `2>&1`).

### Example

```cpp
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << "cerr.tie() == &cout: " << (std::cerr.tie() == &std::cout)
              << '\n'
              << "cerr has unitbuf: "
              << ((std::cerr.flags() & std::ios::unitbuf) != 0) << '\n';

    std::cerr << "diagnostic (goes to stderr, not shown here)\n";
    std::cout << "done\n";
}
```

```text
cerr.tie() == &cout: true
cerr has unitbuf: true
done
```

### See also

- **cout** — the stream `cerr` is tied to
- **clog** — buffered stderr output, not tied to `cout`
- **basic_ostream** — the type `cerr` is an instance of

---
*Source: https://en.cppreference.com/w/cpp/io/cerr*
