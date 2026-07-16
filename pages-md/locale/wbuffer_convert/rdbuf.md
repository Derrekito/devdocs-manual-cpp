# std::wbuffer_convert<Codecvt,Elem,Tr>::rdbuf

```cpp
std::streambuf* rdbuf() const;  // (1)
std::streambuf* rdbuf( std::streambuf* bytebuf );  // (2)
```

1) Returns the pointer to the underlying byte stream.

2) Replaces the associated byte stream with `bytebuf`.

### Return value

1) The current underlying byte stream.

2) The previous byte stream.

### Example

```cpp
#include <codecvt>
#include <iostream>
#include <locale>
#include <sstream>

int main()
{
    // Convert UTF-8 to UCS4
    std::stringbuf utf8buf("z\u00df\u6c34\U0001d10b");
                       // or "\x7a\xc3\x9f\xe6\xb0\xb4\xf0\x9d\x84\x8b"
                       // or u8"zß水𝄋"
    std::wbuffer_convert<std::codecvt_utf8<wchar_t>> conv(&utf8buf);
    std::wistream ucsbuf(&conv);
    std::cout << "Reading from a UTF-8 stringbuf via wbuffer_convert: "
              << std::hex << std::showbase;
    for (wchar_t c; ucsbuf.get(c);)
        std::cout << static_cast<std::wint_t>(c) << ' ';

    // Reuse the same wbuffer_convert to handle UCS4 to UTF-8 output
    conv.rdbuf(std::cout.rdbuf());
    std::wostream out(&conv);
    std::cout << "\nSending UCS4 data to std::cout via wbuffer_convert: ";
    out << L"z\u00df\u6c34\U0001d10b\n";
}
```

Output:

```text
Reading from a UTF-8 stringbuf via wbuffer_convert: 0x7a 0xdf 0x6c34 0x1d10b
Sending UCS4 data to std::cout via wbuffer_convert: zß水𝄋
```

### See also

- **(constructor)** — constructs a new `wbuffer_convert` (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/locale/wbuffer_convert/rdbuf*
