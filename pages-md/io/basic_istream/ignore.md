# std::basic_istream<CharT,Traits>::ignore

`ignore()` reads and throws characters away — it extracts up to
`count` of them, or fewer if it hits `delim` first (which is also
consumed), or fewer still if it hits end-of-file. It's the standard
fix for the classic `std::cin >> n` followed by `std::getline` bug:
`>>` stops right after the number and leaves the trailing `'\n'` sitting
in the stream, so the very next `getline` reads an empty line instead
of the one you wanted. Call
`in.ignore(std::numeric_limits<std::streamsize>::max(), '\n')`
between them to discard everything up to and including that leftover
newline first.

```cpp skip
in.ignore();                    // discard 1 character
in.ignore(n);                   // discard up to n characters
in.ignore(n, '\n');              // ...or stop at '\n', whichever first
in.ignore(std::numeric_limits<std::streamsize>::max(), '\n');  // a whole line
```

### What you provide

- **count** — how many characters to extract at most (default `1`).
  Pass `std::numeric_limits<std::streamsize>::max()` to disable this
  limit and rely on `delim` (or end-of-file) alone to stop.
- **delim** — a character that also stops extraction, once reached; it
  is consumed along with everything before it. Defaults to
  `Traits::eof()`, which can never match a real character, so by
  default only `count` (or end-of-file) stops the call.

### Guarantees and costs

- O(count) — it must extract and discard each character one at a
  time; it does not seek.
- If end-of-file is reached while extracting, `eofbit` is set — this
  is normal for `ignore(max(), '\n')` on the last line and is not an
  error to react to.
- Returns `*this`, so it chains like any other stream operation.
- Behaves as an UnformattedInputFunction: constructs a sentry first,
  and an internal exception is caught and turned into `badbit`
  (rethrown if `badbit` is in the `exceptions()` mask).

### Gotchas

- `ignore()` only discards what's already failed to parse — it does
  nothing about a stream stuck in `failbit`. If the previous
  extraction failed, call `basic_ios::clear` *before* `ignore()`, or
  `ignore()` itself is a no-op on the still-broken stream.
- The default `delim` (`Traits::eof()`) never matches a real
  character, so a bare `in.ignore()` with no arguments discards
  exactly one character, not "until the next separator" — pass an
  explicit `delim` for that.
- Passing a small `count` with no matching `delim` truncates
  silently: it stops after `count` characters even mid-token, with no
  error flag set (unlike `basic_istream::getline`, which sets
  `failbit` on truncation).

### Example

```cpp
#include <iostream>
#include <limits>
#include <sstream>
#include <string>

int main()
{
    std::istringstream in("3\nhello world\n");

    int n;
    in >> n;                     // leaves "\nhello world\n" unread
    in.ignore(std::numeric_limits<std::streamsize>::max(), '\n');

    std::string line;
    std::getline(in, line);

    std::cout << "n=" << n << " line='" << line << "'\n";
}
```

```text
n=3 line='hello world'
```

### Reference

```cpp skip
basic_istream& ignore( std::streamsize count = 1,
                        int_type delim = Traits::eof() );
```

Stops, in order tested, at whichever comes first: `count` characters
extracted (disabled when `count` is
`std::numeric_limits<std::streamsize>::max()`); end-of-file
(`setstate(eofbit)`); or the next character equal to `delim` per
`Traits::eq_int_type` (extracted and discarded; disabled when `delim`
is `Traits::eof()`). LWG 172 (applied retroactively to C++98):
`count`'s type was misspecified as `int`; corrected to `streamsize`.

### See also

- `basic_ios::fail` — check before `ignore()`: a failed stream needs
  `clear()` first
- `basic_ios::eof` — commonly set by an `ignore()` that reaches the
  end while discarding
- `basic_ios::clear` — required before `ignore()` can do anything on
  a failed stream
- `basic_istream::getline` — reads and *keeps* a line instead of
  discarding it
- **getline** — the free function that reads a line into a
  `std::string`
- `basic_istream::get` — extracts characters without discarding them
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/ignore*
