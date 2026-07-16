# std::basic_streambuf<CharT,Traits>::sputc

```cpp
int_type sputc( char_type ch );
```

Writes one character to the output sequence.

If the output sequence write position is not available (the buffer is full),
then calls `overflow(ch)`.

### Parameters

- **ch** — character to write

### Return value

The written character, converted to `int_type` with `Traits::to_int_type(ch)` on
success.

`Traits::eof()` (as returned by `overflow()`) on failure.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::ostringstream s;
    s.rdbuf()->sputc('a');
    std::cout << s.str() << '\n';
}
```

Output:

```text
a
```

### See also

- **sputn** — invokes `xsputn()` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/sputc*
