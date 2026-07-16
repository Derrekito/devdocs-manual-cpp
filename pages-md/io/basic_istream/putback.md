# std::basic_istream<CharT,Traits>::putback

```cpp
basic_istream& putback( char_type ch );
```

Puts the character `ch` back to the input stream so the next extracted character
will be `ch`.

First clears `eofbit`, then behaves as UnformattedInputFunction. After
constructing and checking the sentry object, if `rdbuf()` is not null, calls
`rdbuf()->sputbackc(ch)`, which calls `rdbuf()->pbackfail(ch)` if `ch` does not
equal the most recently extracted character.

If `rdbuf()` is null or if `rdbuf->sputbackc(ch)` returns `Traits::eof()`, calls
`setstate(badbit)`.

In any case, sets the `gcount()` counter to zero.

### Parameters

- **ch** — character to put into input stream

### Return value

`*this`

### Exceptions

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Example

Demonstrates the difference between modifying and non-modifying `putback()`.

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::stringstream s1("Hello, world"); // IO stream
    s1.get();
    if (s1.putback('Y')) // modifies the buffer
        std::cout << s1.rdbuf() << '\n';
    else
        std::cout << "putback failed\n";

    std::cout << "--\n";

    std::istringstream s2("Hello, world"); // input-only stream
    s2.get();
    if (s2.putback('Y')) // cannot modify input-only buffer
        std::cout << s2.rdbuf() << '\n';
    else
        std::cout << "putback failed\n";
    s2.clear();

    std::cout << "--\n";

    if (s2.putback('H')) // non-modifying putback
        std::cout << s2.rdbuf() << '\n';
    else
        std::cout << "putback failed\n";
}
```

Output:

```text
Yello, world
--
putback failed
--
Hello, world
```

### See also

- **sputbackc** — puts one character back in the input sequence (public member
  function of `std::basic_streambuf<CharT,Traits>`)
- **unget** — unextracts a character (public member function)
- **peek** — reads the next character without extracting it (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/putback*
