# operator<<(std::basic_string_view)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                std::basic_string_view<CharT, Traits> v );  // (since C++17)
```

Behaves as a FormattedOutputFunction. After constructing and checking the sentry
object, determines the output format padding.

Then stores each character from the resulting sequence `seq` (the contents of
`v` with padding) to the output stream `os` as if by calling
`os.rdbuf()->sputn(seq, n)`, where `n` is `std::max(os.width(), str.size())`.

Finally, calls `os.width(0)` to cancel the effects of `std::setw`, if any.

### Exceptions

May throw `std::ios_base::failure` if an exception is thrown during output.

### Parameters

- **os** — a character output stream
- **v** — the view to be inserted

### Return value

`os`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view s{"abc"};
    constexpr int width{5};

    // fill/left/right properties are kept until changed
    std::cout << std::setfill('-');
    std::cout << std::left;

    std::cout << '[' << std::setw(width) << s << "]\n";
    std::cout << '[' << std::setw(width) << s << "]\n";

    std::cout << std::right;
    std::cout << '[' << std::setw(width) << s << "]\n";

    // width is reset after each call
    std::cout << '[' << s << "]\n";
}
```

Output:

```text
[abc--]
[abc--]
[--abc]
[abc]
```

### See also

- **operator<<operator>>** — performs stream input and output on strings
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/operator_ltlt*
