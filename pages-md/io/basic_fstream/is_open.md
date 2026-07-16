# std::basic_fstream<CharT,Traits>::is_open

```cpp
bool is_open() const;
```

Checks if the file stream has an associated file.

Effectively calls `rdbuf()->is_open()`.

### Parameters

(none)

### Return value

`true` if the file stream has an associated file, `false` otherwise.

### Example

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    std::string filename = "some_file";

    std::fstream fs(filename, std::ios::in);

    std::cout << std::boolalpha;
    std::cout << "fs.is_open() = " << fs.is_open() << '\n';

    if (!fs.is_open())
    {
        fs.clear();
        fs.open(filename, std::ios::out);
        std::cout << "fs.is_open() = " << fs.is_open() << '\n';
    }
}
```

Possible output:

```text
fs.is_open() = false
fs.is_open() = true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 365 | C++98 | `is_open` was not declared with const qualifier | declared
      with const qualifier

### See also

- **open** — opens a file and associates it with the stream (public member
  function)
- **close** — closes the associated file (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_fstream/is_open*
