# std::basic_istream<CharT,Traits>::peek

```cpp
int_type peek();
```

Behaves as UnformattedInputFunction. After constructing and testing the sentry
object, reads the next character from the input stream without extracting it.

### Parameters

(none)

### Return value

If `good() == true`, returns the next character as obtained by
`rdbuf()->sgetc()`.

Otherwise, returns `Traits::eof()`.

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
    char c1 = s1.peek();
    char c2 = s1.get();
    std::cout << "Peeked: " << c1 << " got: " << c2 << '\n';
}
```

Output:

```text
Peeked: H got: H
```

### See also

- **sgetc** — reads one character from the input sequence without advancing the
  sequence (public member function of `std::basic_streambuf<CharT,Traits>`)
- **get** — extracts characters (public member function)
- **unget** — unextracts a character (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/peek*
