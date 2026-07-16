# std::setw

Sets the minimum field width for the **next** formatted insertion or
extraction only. Read that twice: it is not sticky. After one `<<` or
`>>` uses it, the width silently resets to 0 — call `setw` again
before every field that needs it.

```cpp skip
out << std::setw(n) << value;   // pads value to at least n chars
in  >> std::setw(n) >> arr;     // limits extraction to n-1 chars + '\0'
```

### What you provide

- **n** — the minimum field width, as an `int`.

### Guarantees and costs

- Equivalent to calling `str.width(n)` on the stream's `ios_base`.
- Applies to exactly one insertion or extraction, then the width
  resets to 0 ("unspecified"), which formatting code treats as "no
  padding". Which operations consume it is defined per `operator<<`
  and `operator>>` overload, not by `setw` itself.
- Padding uses the current fill character (see `std::setfill`,
  default space) and the current adjustment (`std::left`/`right`,
  default right for most types).
- On extraction into a `char` array, `n` bounds the total characters
  written including the terminating `'\0'` — a critical overflow
  guard for `>>` into fixed buffers.
- Usable with any character stream (narrow or wide), not just
  `std::ostream`/`std::istream` (LWG 183).

### Gotchas

- Forgetting it resets: `out << setw(6) << a << b;` pads only `a`;
  `b` prints at its natural width.
- Multi-field alignment (tables, columns) means calling `setw` before
  *each* field, every time.
- `setw` alone doesn't affect strings differently from numbers, but
  the *direction* of padding does — set `std::left` explicitly if you
  want left-justified text; the default is right-justified.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>

int main()
{
    std::cout << "no setw:   [" << 42 << "]\n"
              << "setw(6):   [" << std::setw(6) << 42 << "]\n"
              << "resets:    [" << 89 << "][" << std::setw(6) << 12
              << "][" << 34 << "]\n";

    std::istringstream is("hello world");
    char arr[6];
    is >> std::setw(6) >> arr;   // at most 5 chars + '\0'
    std::cout << "bounded read: \"" << arr << "\"\n";
}
```

```text
no setw:   [42]
setw(6):   [    42]
resets:    [89][    12][34]
bounded read: "hello"
```

### Reference

```cpp skip
/* unspecified */ setw( int n );
```

Returns an object of unspecified type such that, when written to an
output stream or read from an input stream, it calls `str.width(n)`
on the stream and otherwise behaves like the stream itself.

### See also

- **width** — the underlying `ios_base` member `setw` calls
- **setfill** — changes the fill character used for padding
- **left**, **right** — set the padding side

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setw*
