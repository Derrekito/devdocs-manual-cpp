# std::basic_streambuf<CharT,Traits>::sungetc

```cpp
int_type sungetc();
```

If a putback position is available in the get area (`gptr() > eback()`), then
decrements the next pointer (`gptr()`) and returns the character it now points
to.

If a putback position is not available, then calls `pbackfail()` to back up the
input sequence if possible.

The I/O stream function `basic_istream::unget` is implemented in terms of this
function.

### Parameters

(none)

### Return value

If putback position was available, returns the character that the next pointer
is now pointing at, converted to `int_type` with `Traits::to_int_type(*gptr())`.
The next single-character input from this streambuf will return this character.

If putback position was not available, returns what `pbackfail()` returns, which
is `Traits::eof()` on failure.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::stringstream s("abcdef"); // gptr() points to 'a'
    char c1 = s.get(); // c = 'a', gptr() now points to 'b'
    char c2 = s.rdbuf()->sungetc(); // same as s.unget(): gptr() points to 'a' again
    char c3 = s.get(); // c3 = 'a', gptr() now points to 'b'
    char c4 = s.get(); // c4 = 'b', gptr() now points to 'c'
    std::cout << c1 << c2 << c3 << c4 << '\n';

    s.rdbuf()->sungetc();  // back to 'b'
    s.rdbuf()->sungetc();  // back to 'a'
    int eof = s.rdbuf()->sungetc();  // nothing to unget: pbackfail() fails
    if (eof == EOF)
            std::cout << "Nothing to unget after 'a'\n";
}
```

Output:

```text
aaab
Nothing to unget after 'a'
```

### See also

- **sputbackc** — puts one character back in the input sequence (public member
  function)
- **unget** — unextracts a character (public member function of
  `std::basic_istream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/sungetc*
