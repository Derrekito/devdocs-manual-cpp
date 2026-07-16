# std::getline

`std::getline` reads characters from an input stream into a
`std::string`, stopping at (and consuming) a delimiter — `'\n'` by
default. It's the standard way to read a whole line, including
embedded spaces that `operator>>` would stop at.

```cpp skip
std::getline(input, str);                     // read up to '\n', discard it
std::getline(input, str, delim);              // read up to a custom delimiter
std::getline(std::move(input), str);          // rvalue-stream overload (since C++11)
std::getline(std::move(input), str, delim);   // rvalue-stream overload (since C++11)
```

### What you provide

- **input** — the stream to read from: an lvalue, or (since C++11) an
  rvalue `std::basic_istream` such as `std::cin`, an `std::ifstream`,
  or an `std::istringstream`.
- **str** — the `std::string` (or other `basic_string` instantiation)
  that receives the characters. Its previous contents are erased
  first, even if the read that follows immediately fails.
- **delim** — the character that ends the read. It's consumed from the
  stream but not appended to `str`. Omit it to default to
  `input.widen('\n')` — `'\n'` for a plain `char` stream.

### Guarantees and costs

- Behaves as an UnformattedInputFunction, except `input.gcount()` is
  left unaffected.
- Extraction stops, in order, at: end-of-file (sets `eofbit`), the
  delimiter (consumed, not stored), or `str.max_size()` characters
  read (sets `failbit`).
- If nothing was extracted at all — not even a discarded delimiter —
  `failbit` is set. The idiomatic loop is `while (std::getline(in, line))`,
  which relies on exactly this to terminate.
- Returns `input`, so the call itself is usable as a stream-testing
  condition.

### Gotchas

- `str` is cleared *before* reading starts — a failed call still wipes
  out whatever it held.
- The rvalue-stream overloads (C++11) exist so `getline` can be called
  directly on a temporary, e.g.
  `std::getline(std::ifstream("f"), line)`, without binding it to a
  named variable first.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::istringstream in{"line one\nline two\n"};
    std::string line;

    std::getline(in, line);
    std::cout << "first: " << line << '\n';

    std::getline(in, line, 'o');   // custom delimiter
    std::cout << "next up to 'o': " << line << '\n';
}
```

```text
first: line one
next up to 'o': line tw
```

### Reference

```cpp skip
template< class CharT, class Traits, class Allocator >
std::basic_istream<CharT, Traits>&
getline( std::basic_istream<CharT, Traits>& input,
         std::basic_string<CharT, Traits, Allocator>& str, CharT delim );  // (1)
template< class CharT, class Traits, class Allocator >
std::basic_istream<CharT, Traits>&
getline( std::basic_istream<CharT, Traits>&& input,
         std::basic_string<CharT, Traits, Allocator>& str, CharT delim );  // (2) (since C++11)
template< class CharT, class Traits, class Allocator >
std::basic_istream<CharT, Traits>&
getline( std::basic_istream<CharT, Traits>& input,
         std::basic_string<CharT, Traits, Allocator>& str );  // (3)
template< class CharT, class Traits, class Allocator >
std::basic_istream<CharT, Traits>&
getline( std::basic_istream<CharT, Traits>&& input,
         std::basic_string<CharT, Traits, Allocator>& str );  // (4) (since C++11)
```

(3,4) are equivalent to calling (1,2) with `input.widen('\n')` as
`delim`. Defect report LWG 91 (C++98) corrected the original wording,
which didn't actually specify `getline` as an UnformattedInputFunction.

### See also

- **basic_istream::getline** — the member-function counterpart; reads
  into a raw `char` buffer of a given size instead of a `std::string`

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/getline*
