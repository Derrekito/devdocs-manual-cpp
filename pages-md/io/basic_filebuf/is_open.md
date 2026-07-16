# std::basic_filebuf<CharT,Traits>::is_open

```cpp
bool is_open() const;
```

Returns `true` if the most recent call to `open()` succeeded and there has been
no call to `close()` since then.

### Parameters

(none)

### Return value

`true` if the associated file is open, `false` otherwise.

### Notes

This function is typically called by `std::basic_fstream::is_open()`.

### Example

```cpp
#include <fstream>
#include <iostream>

int main()
{
    std::ifstream fs("test.txt");
    std::filebuf fb;
    fb.open("test.txt", std::ios_base::in);
    std::cout << std::boolalpha
              << "direct call: " << fb.is_open() << '\n'
              << "through streambuf: " << fs.rdbuf()->is_open() << '\n'
              << "through fstream: " << fs.is_open() << '\n';
}
```

Output:

```text
direct call: true
through streambuf: true
through fstream: true
```

### See also

- **open** — opens a file and configures it as the associated character sequence
  (public member function)
- **close** — flushes the put area buffer and closes the associated file (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_filebuf/is_open*
