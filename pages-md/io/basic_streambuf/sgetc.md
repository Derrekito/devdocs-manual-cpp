# std::basic_streambuf<CharT,Traits>::sgetc

```cpp
int_type sgetc();
```

Reads one character from the input sequence.

If the input sequence read position is not available, returns `underflow()`.
Otherwise returns `Traits::to_int_type(*gptr())`.

### Parameters

(none)

### Return value

The value of the character pointed to by the *get pointer*.

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    std::stringstream stream("Hello, world");
    std::cout << "sgetc() returned '" << (char)stream.rdbuf()->sgetc() << "'\n";
    std::cout << "peek() returned '" << (char)stream.peek() << "'\n";
    std::cout << "get() returned '" << (char)stream.get() << "'\n";
}
```

Output:

```text
sgetc() returned 'H'
peek() returned 'H'
get() returned 'H'
```

### See also

- **sbumpcstossc (removed in C++17)** — reads one character from the input
  sequence and advances the sequence (public member function)
- **snextc** — advances the input sequence, then reads one character without
  advancing again (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_streambuf/sgetc*
