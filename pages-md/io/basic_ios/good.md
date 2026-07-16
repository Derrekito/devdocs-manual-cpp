# std::basic_ios<CharT,Traits>::good

`good()` is **not** the opposite of `fail()`. It requires every flag
clear — `eofbit`, `failbit`, and `badbit` all false — so it also
returns false once `eof()` is true, even when nothing actually went
wrong: a read that successfully consumes the last byte of input can
leave `eofbit` set (see `basic_ios::eof`), and `good()` reports that
as failure. That's why the idiomatic check after a read is `if (in)`
or `while (in >> x)`, which use `operator bool` — defined as
`!fail()`, not `good()`. Reserve `good()` for a true zero-flags check,
such as right after opening a stream, not as a loop condition.

```cpp skip
if (in)                // idiomatic: operator bool is !fail()
if (in.good())           // stricter: false once eofbit is set too
```

### What you provide

No parameters. Call it when you specifically need "no flags at all
are set", not as a stand-in for "did the last read work".

### Guarantees and costs

- O(1); equivalent to `rdstate() == 0`.

### Gotchas

- `good()` false does not mean the last read failed to produce a
  value — it can just mean you're now at end-of-file after a
  perfectly good read.
- Never use `good()` as a loop condition (`while (in.good())`): it has
  the same off-by-one trap as `while (!in.eof())`, because `eofbit`
  can be set on the very read that still succeeded.
- The right tool for "did that operation work" is `operator bool` /
  `!fail()`, i.e. plain `if (in)` — see `basic_ios::fail`.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream in("42");
    std::cout << std::boolalpha
               << "before read: good=" << in.good() << '\n';

    int n;
    in >> n;   // reads "42"; parsing must peek past it and finds nothing

    std::cout << "after read:  good=" << in.good()
               << " fail=" << in.fail() << " n=" << n << '\n';
}
```

```text
before read: good=true
after read:  good=false fail=false n=42
```

### Reference

```cpp skip
bool good() const;
```

Equivalent to `rdstate() == 0`. See `ios_base::iostate` for the flags;
`basic_ios::fail` carries the full accessor-combination table for
this cluster.

### See also

- `basic_ios::fail` — the check to use after a read; not `!good()`
- `basic_ios::eof` — the flag that most often makes `good()` false
  even on success
- `basic_ios::clear` — resets the state so the stream works again
- `basic_istream::ignore` — skip past bad input before retrying
- `basic_istream::getline` — a char-array read subject to the same
  `good()` trap

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/good*
