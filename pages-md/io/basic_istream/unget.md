# std::basic_istream<CharT,Traits>::unget

```cpp
basic_istream& unget();
```

Makes the most recently extracted character available again.

First, clears `eofbit`. Then,(since C++11) `unget` behaves as
UnformattedInputFunction. After constructing and checking the sentry object, if
any `ios_base::iostate` flags are set, the function sets `failbit` and returns.
Otherwise, calls `rdbuf()->sungetc()`.

If `rdbuf()->sungetc()` returns `Traits::eof()`, calls `setstate(badbit)`.

In any case, sets the `gcount()` counter to zero.

### Parameters

(none)

### Return value

`*this`

### Exceptions

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream s1("Hello, world.");
    char c1 = s1.get();
    if (s1.unget())
    {
        char c2 = s1.get();
        std::cout << "Got: '" << c1 << "'. Got again: '" << c2 << "'.\n";
    }
}
```

Output:

```text
Got: 'H'. Got again: 'H'.
```

### See also

- **sungetc** — moves the next pointer in the input sequence back by one (public
  member function of `std::basic_streambuf<CharT,Traits>`)
- **get** — extracts characters (public member function)
- **peek** — reads the next character without extracting it (public member
  function)
- **putback** — puts a character into input stream (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/unget*
