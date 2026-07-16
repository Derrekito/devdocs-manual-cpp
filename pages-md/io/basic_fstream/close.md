# std::basic_fstream<CharT,Traits>::close

```cpp
void close();
```

Closes the associated file.

Effectively calls `rdbuf()->close()`. If an error occurs during operation,
`setstate(failbit)` is called.

### Parameters

(none)

### Return value

(none)

### Notes

This function is called by the destructor of `basic_fstream` when the stream
object goes out of scope and is not usually invoked directly.

### Example

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    std::fstream f1("example1", std::ios::out),
                 f2("example2", std::ios::out),
                 f3("example3", std::ios::out);

    std::cout << std::boolalpha
              << f1.is_open() << '\n'
              << f2.is_open() << '\n'
              << f3.is_open() << '\n';

    f1.close();
    f2.close();

    std::cout << f1.is_open() << '\n'
              << f2.is_open() << '\n'
              << f3.is_open() << '\n';
}
```

Possible output:

```text
true
true
true
false
false
true
```

### See also

- **is_open** — checks if the stream has an associated file (public member
  function)
- **open** — opens a file and associates it with the stream (public member
  function)
- **close** — flushes the put area buffer and closes the associated file (public
  member function of `std::basic_filebuf<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_fstream/close*
