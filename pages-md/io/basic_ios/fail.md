# std::basic_ios<CharT,Traits>::fail

`fail()` is true once a read has failed to deliver a value — that is,
once `failbit` or `badbit` is set. This is the question you actually
want answered after `in >> x` or similar ("did that work?"), and it's
what the stream's own boolean conversion tests: `operator bool` is
defined as `!fail()`, so `if (in)` and `if (!in.fail())` are the same
check. One surprise: `eofbit` on its own, with neither `failbit` nor
`badbit` set, does **not** make `fail()` true — reaching the end while
still successfully extracting a value is not a failure (see
`basic_ios::eof`).

```cpp skip
if (!(in >> x))      // idiomatic: operator bool is !fail()
if (in.fail())        // the same check, spelled out
if (in.bad())          // narrower: only the unrecoverable subset
```

### What you provide

No parameters. Call it right after the operation whose outcome you
want to know, before anything else touches the stream.

### Guarantees and costs

- O(1); equivalent to `(rdstate() & (failbit | badbit)) != 0`.
- `operator bool` is `!fail()` and `operator!` is `fail()` — that
  identity is why idiomatic loops write `while (in >> x)` rather than
  calling `fail()` explicitly.

### Gotchas

- `eofbit` alone does not set `fail()`: a read that exhausts the input
  while still producing a valid value leaves `fail()` false even
  though `eof()` is now true.
- Once `fail()` is true the stream is sticky — every further
  formatted or unformatted operation is a no-op until `clear()` resets
  the state (see `basic_ios::clear`).
- `fail()` alone doesn't say *which* problem occurred: check `bad()`
  to tell an irrecoverable I/O error apart from a mere formatting
  mismatch.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream in("10 20 notanumber");
    int sum = 0, count = 0;

    for (int n; in >> n; ++count)
        sum += n;

    std::cout << "read " << count << " values, sum = " << sum << '\n';
    std::cout << std::boolalpha << "fail=" << in.fail()
               << " eof=" << in.eof() << " bad=" << in.bad() << '\n';
}
```

```text
read 2 values, sum = 30
fail=true eof=false bad=false
```

### Reference

```cpp skip
bool fail() const;
```

Equivalent to `rdstate() & (failbit | badbit) != 0`. This table gives
every combination of the three `ios_base::iostate` flags and how each
`basic_ios` accessor reads them — the reference for the whole cluster:

  eofbit | failbit | badbit | good() | fail() | bad() | eof() | bool | !
  ------ | ------- | ------ | ------ | ------ | ------| ----- | ---- | -
  F | F | F | T | F | F | F | T | F
  F | F | T | F | T | T | F | F | T
  F | T | F | F | T | F | F | F | T
  F | T | T | F | T | T | F | F | T
  T | F | F | F | F | F | T | T | F
  T | F | T | F | T | T | T | F | T
  T | T | F | F | T | F | T | F | T
  T | T | T | F | T | T | T | F | T

### See also

- `basic_ios::eof` — true once a read has hit the end; not the same
  test as `fail()`
- `basic_ios::good` — true only when every flag is clear, stricter
  than `!fail()`
- `basic_ios::clear` — resets the state so the stream works again
- `basic_istream::ignore` — the usual next step: discard bad input
  before retrying
- `basic_istream::getline` — the char-array read that also sets
  `failbit` on overflow
- **ferror** — checks for a file error (function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/fail*
