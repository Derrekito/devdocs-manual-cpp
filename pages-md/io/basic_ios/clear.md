# std::basic_ios<CharT,Traits>::clear

`clear()` **sets** the stream's error state to whatever you pass it —
it does not merely turn bits off. Called with no argument it assigns
`goodbit`, which has the effect of clearing every flag, and that's
the call you need before a stream can be read from again after
`fail()` became true: once `failbit` or `badbit` is set, every further
read is a silent no-op until something resets the state, and `clear()`
is that something. The standard recovery recipe pairs it with
`basic_istream::ignore`: `clear()` to unstick the stream, then
`ignore(numeric_limits<streamsize>::max(), '\n')` to throw away the
bad token so the next read doesn't immediately fail again on the same
text.

```cpp skip
in.clear();                                 // reset to goodbit (no error)
in.clear(std::ios_base::eofbit);            // set to a specific state
in.clear(in.rdstate() & ~std::ios_base::failbit);  // drop just one flag
```

### What you provide

- **state** — the new state, defaulting to `goodbit`. It can be
  `goodbit` (no error), `badbit` (irrecoverable), `failbit`
  (formatting/extraction failure), or `eofbit` (end reached), OR'd
  together. Whatever you pass *becomes* `rdstate()` — it replaces the
  old flags rather than adding to or subtracting from them.

### Guarantees and costs

- O(1).
- If `rdbuf()` is null (no associated stream buffer), `badbit` is
  forced on regardless of the `state` you pass — `state | badbit` is
  assigned instead.
- If the new state includes a bit that is also set in the stream's
  `exceptions()` mask, `clear()` throws `std::ios_base::failure`.

### Gotchas

- `clear()` alone doesn't discard the bad input still sitting in the
  buffer — it only resets the flags. Follow it with
  `basic_istream::ignore` (or read/discard the offending text some
  other way), or the next extraction fails again on the same
  characters.
- To clear only one flag and keep the rest, read `rdstate()` first and
  mask it: `in.clear(in.rdstate() & ~std::ios_base::failbit)`. Calling
  `clear()` bare wipes everything, including `eofbit` you may still
  want.
- Since C++11, a successful `seekg()`/`tellg()` clears `eofbit` itself
  as its first step — but only if the stream isn't already in a
  failed state, so `clear()` still has to run first when `failbit` is
  also set.

### Example

```cpp
#include <iostream>
#include <limits>
#include <sstream>

int main()
{
    std::istringstream in("abc 3.14");
    double n;

    while (!(in >> n))
    {
        in.clear();                          // unstick the stream
        in.ignore(std::numeric_limits<std::streamsize>::max(), ' ');
        std::cout << "skipped bad token\n";
    }
    std::cout << "parsed " << n << '\n';
}
```

```text
skipped bad token
parsed 3.14
```

### Reference

```cpp skip
void clear( std::ios_base::iostate state = std::ios_base::goodbit );
```

`state` combines `goodbit`, `badbit`, `failbit`, `eofbit`. LWG 412
(applied retroactively to C++98): originally an exception was thrown
if the *current* error state intersected the `exceptions()` mask;
corrected so it's the *new* state that's checked instead.

### See also

- `basic_ios::fail` — the check `clear()` exists to unstick
- `basic_ios::eof` — the flag most often left set after `clear()`'s
  default call
- `basic_ios::good` — true immediately after a bare `clear()`
- `basic_istream::ignore` — pairs with `clear()` in the standard
  recovery recipe
- `basic_istream::getline` — another read that needs this same
  recovery after overflow
- `basic_ios::setstate` — sets state flags without resetting others
  (public member function)
- `basic_ios::rdstate` — returns the current state flags (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/clear*
