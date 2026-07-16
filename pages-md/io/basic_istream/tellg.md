# std::basic_istream<CharT,Traits>::tellg

```cpp
pos_type tellg();
```

Returns input position indicator of the current associated `streambuf` object.

Behaves as UnformattedInputFunction, except that `gcount()` is not affected.
After constructing and checking the sentry object, if `fail() == true`, returns
`pos_type(-1)`. Otherwise, returns `rdbuf()->pubseekoff(0, std::ios_base::cur,
std::ios_base::in)`.

### Parameters

(none)

### Return value

The current position of the get pointer on success, `pos_type(-1)` on failure.

### Exceptions

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Example

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::string str = "Hello, world";
    std::istringstream in(str);
    std::string word;
    in >> word;
    std::cout << "After reading the word \"" << word
              << "\" tellg() returns " << in.tellg() << '\n';
}
```

Output:

```text
After reading the word "Hello," tellg() returns 6
```

### See also

- **seekoff [virtual]** — repositions the file position, using relative
  addressing (virtual protected member function of
  `std::basic_filebuf<CharT,Traits>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)
- **seekoff [virtual]** — repositions the next pointer in the input sequence,
  output sequence, or both, using relative addressing (virtual protected member
  function of `std::strstreambuf`)
- **seekg** — sets the input position indicator (public member function)
- **tellp** — returns the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)
- **seekp** — sets the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/tellg*
