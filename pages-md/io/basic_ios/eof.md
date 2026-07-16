# std::basic_ios<CharT,Traits>::eof

`eof()` is true **after** a read has already hit the end of the input
— not before it. It only reports what the *last* operation found, so
the read that consumes the final line still returns success, and
`eof()` stays false; only the *next* read, which finds nothing, sets
`eofbit`. That lag is why `while (!in.eof())` is a classic bug: the
loop body still runs once more after the last good read, processing
stale data (or garbage from an untouched variable), and only stops
when the following read fails. Loop on the read itself instead —
`while (in >> x)` or `while (std::getline(in, line))` — its own
success or failure is exactly what you want to test.

```cpp skip
if (in.eof())                 // true only after a read past the end
while (in >> x) { ... }       // correct: stops exactly when nothing's left
while (!in.eof()) { ... }     // WRONG: one stale extra iteration
```

### What you provide

No parameters. Call it after a read fails, to check whether "ran out
of input" was the reason (as opposed to a formatting error).

### Guarantees and costs

- O(1); equivalent to `rdstate() & eofbit`.
- Reports only the state left by the most recent I/O operation — it
  never probes the underlying source on its own.

### Gotchas

- `eof()` can also flip to true as a side effect of a *successful*
  extraction: parsing a number needs to peek one character past its
  last digit to know it's finished, and if that peek finds nothing,
  `eofbit` is set even though the value was read correctly and
  `fail()` stays false. This is still "after a read hit the end", just
  the same call that succeeded.
- `eof()` true does not mean the last read failed — check `fail()` for
  that (see `basic_ios::fail`).
- Don't reset just `eofbit` to "try again" at the same spot: since
  C++11 `seekg()`/`tellg()` clear `eofbit` themselves as their first
  step, but only once the stream isn't in a failed state — call
  `basic_ios::clear` first if `failbit` is also set.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::istringstream in("line1\nline2\n");
    std::string line;
    int lines = 0;

    while (std::getline(in, line))
    {
        ++lines;
        std::cout << "read '" << line << "', eof=" << std::boolalpha
                   << in.eof() << '\n';
    }
    std::cout << "loop exited after " << lines << " lines, eof="
               << in.eof() << '\n';
}
```

```text
read 'line1', eof=false
read 'line2', eof=false
loop exited after 2 lines, eof=true
```

### Reference

```cpp skip
bool eof() const;
```

Equivalent to `rdstate() & eofbit`. See `ios_base::iostate` for every
flag; `basic_ios::fail` carries the full accessor-combination table
for this cluster.

### See also

- `basic_ios::fail` — the check that actually answers "did it work?"
- `basic_ios::good` — true only when every flag, including `eofbit`,
  is clear
- `basic_ios::clear` — resets the state so the stream works again
- `basic_istream::ignore` — skip past bad input before retrying
- `basic_istream::getline` — the char-array read this loop pattern
  also applies to
- **feof** — checks for the end-of-file (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/eof*
