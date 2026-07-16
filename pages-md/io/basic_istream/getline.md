# std::basic_istream<CharT,Traits>::getline

This is the char-array member: it reads a line into a raw buffer you
already own, given its size. For a `std::string` that grows to fit,
use the free function **getline** instead (`std::getline(in, str)`) —
that's almost always the right choice in ordinary code; reach for this
member when you specifically need a fixed buffer. Two facts define
its behavior: it stops storing after `count - 1` characters, always
leaving room for the trailing `'\0'`, and if it has to stop for that
reason — the line didn't fit — it sets `failbit`, unlike hitting the
delimiter or end-of-file, which are treated as ordinary termination.

```cpp skip
in.getline(buf, count);            // read up to '\n', discard it, stop
in.getline(buf, count, delim);     // ...or stop at a custom delimiter
```

### What you provide

- **s** — pointer to the character array to fill.
- **count** — the array's size. At most `count - 1` characters are
  stored, leaving room for the null terminator that's always written.
- **delim** — the character that ends the line; consumed from the
  stream but not stored. Defaults to `'\n'` (via `widen('\n')`).

### Guarantees and costs

- O(count) in the worst case; stops as soon as `delim` or end-of-file
  is reached.
- Always null-terminates the buffer (writes `CharT()`) when
  `count > 0`, even if zero characters were read.
- Stops, in this order: end-of-file; the delimiter (extracted and
  counted in `gcount()`, but not stored); or `count - 1` characters
  extracted with no delimiter found — this last case alone sets
  `failbit`.
- Because the delimiter check happens before the length check, a line
  that fits exactly does *not* trigger `failbit`. Because the
  delimiter itself counts as extracted, an empty line doesn't either.
- If it extracts no characters at all, `failbit` is set regardless of
  why.

### Gotchas

- A too-long line still leaves a truncated, valid, null-terminated
  string in `s` — `failbit` is set, but the buffer isn't left empty or
  garbage. Check `fail()` (after `basic_ios::clear`) if you need to
  know whether truncation happened before reading on.
- After truncation, the rest of that line is still sitting unread in
  the stream — the next `getline` call reads its leftover tail, not
  the next line, unless you discard it first (see
  `basic_istream::ignore`).
- Confusing this with the free function **getline** is the single
  most common mixup: this one takes `(char*, size)` and never touches
  a `std::string`; that one takes `(istream&, string&)` and resizes
  automatically.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream in("hi\ntoolongforthebuffer\n");
    char buf[8];

    in.getline(buf, sizeof buf);
    std::cout << "line1='" << buf << "' fail=" << std::boolalpha
               << in.fail() << '\n';

    in.getline(buf, sizeof buf);
    std::cout << "line2='" << buf << "' fail=" << in.fail() << '\n';
}
```

```text
line1='hi' fail=false
line2='toolong' fail=true
```

### Reference

```cpp skip
basic_istream& getline( char_type* s, std::streamsize count );  // (1)
basic_istream& getline( char_type* s, std::streamsize count,
                         char_type delim );  // (2)
```

(1) is equivalent to `getline(s, count, widen('\n'))`. Behaves as an
UnformattedInputFunction. LWG 531 (applied retroactively to C++98):
originally unspecified what happened when `count` was non-positive;
corrected so that no character is extracted in that case.

### See also

- **getline** — the free function that reads into a `std::string`
  instead of a raw buffer
- `basic_istream::ignore` — discard a truncated line's leftover tail
  before the next read
- `basic_ios::fail` — the check for whether truncation (or anything
  else) happened
- `basic_ios::eof` — set if end-of-file, rather than the delimiter,
  ended the line
- `basic_ios::clear` — required before reading again after a
  `failbit` from truncation
- `basic_istream::get` — extracts characters without needing a
  delimiter to stop (public member function)
- `basic_istream::read` — extracts a fixed block of characters
  (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/getline*
