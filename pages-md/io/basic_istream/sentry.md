# std::basic_istream<CharT,Traits>::sentry

```cpp
class sentry;
```

An object of class `basic_istream::sentry` is constructed in local scope at the
beginning of each member function of `std::basic_istream` that performs input
(both formatted and unformatted). Its constructor prepares the input stream:
checks if the stream is already in a failed state, flushes the tie()'d output
streams, skips leading whitespace unless `noskipws` flag is set, and performs
other implementation-defined tasks if necessary. All cleanup, if necessary, is
performed in the destructor, so that it is guaranteed to happen if exceptions
are thrown during input.

### Member types

- **`traits_type`** — `Traits`

### Member functions

- ****(constructor)**** — constructs the sentry object. All the preparation
  tasks are done here (public member function)
- ****(destructor)**** — finalizes the stream object after formatted input or
  after exception, if necessary (public member function)
- **operator= [deleted]** — not copy assignable (public member function)
- ****operator bool**** — checks if the preparation of the stream object was
  successful (public member function)

## std::basic_istream::sentry::sentry

```cpp
explicit sentry( std::basic_istream<CharT, Traits>& is, bool noskipws = false );
```

Prepares the stream for formatted input.

If `is.good()` is `false`, calls `is.setstate(std::ios_base::failbit)` and
returns. Otherwise, if `is.tie()` is not a null pointer, calls
`is.tie()->flush()` to synchronize the output sequence with external streams.
This call can be suppressed if the put area of `is.tie()` is empty. The
implementation may defer the call to `flush()` until a call of
`is.rdbuf()->underflow()` occurs. If no such call occurs before the sentry
object is destroyed, it may be eliminated entirely.

If `noskipws` is zero and `is.flags() & std::ios_base::skipws` is nonzero, the
function extracts and discards all whitespace characters until the next
available character is not a whitespace character (as determined by the
currently imbued locale in `is`). If `is.rdbuf()->sbumpc()` or
`is.rdbuf()->sgetc()` returns `traits::eof()`, the function calls
`setstate(std::ios_base::failbit | std::ios_base::eofbit)` (which may throw
`std::ios_base::failure`).

Additional implementation-defined preparation may take place, which may call
`setstate(std::ios_base::failbit)` (which may throw `std::ios_base::failure`).

If after preparation is completed, `is.good() == true`, then any subsequent
calls to operator bool will return `true`.

### Parameters

- **is** — input stream to prepare
- **noskipws** — `true` if whitespace should not be skipped

### Exceptions

`std::ios_base::failure` if the end of file condition occurs when skipping
whitespace.

## std::basic_istream::sentry::~sentry

```cpp
~sentry();
```

Does nothing.

## std::basic_istream::sentry::operator bool

```cpp
explicit operator bool() const;
```

Checks whether the preparation of the input stream was successful.

### Parameters

(none)

### Return value

`true` if the initialization of the input stream was successful, `false`
otherwise.

### Example

```cpp
#include <iostream>
#include <sstream>

struct Foo
{
    char n[5];
};

std::istream& operator>>(std::istream& is, Foo& f)
{
    std::istream::sentry s(is);
    if (s)
        is.read(f.n, 5);
    return is;
}

int main()
{
    std::string input = "   abcde";
    std::istringstream stream(input);
    Foo f;
    stream >> f;
    std::cout.write(f.n, 5);
    std::cout << '\n';
}
```

Output:

```text
abcde
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 195 | C++98 | it was unclear whether the constructor would set `eofbit` |
      made clear
  LWG 419 | C++98 | the constructor did not set `failbit` if `eofbit` has been
      set | sets `failbit` in this case

### See also

- **operator>>** — extracts formatted data (public member function)
- **operator>>(std::basic_istream)** — extracts characters and character arrays
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/sentry*
