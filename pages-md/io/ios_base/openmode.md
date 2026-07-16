# std::ios_base::openmode

```cpp
typedef /* implementation defined */ openmode;
static constexpr openmode app       = /* implementation defined */;
static constexpr openmode binary    = /* implementation defined */;
static constexpr openmode in        = /* implementation defined */;
static constexpr openmode out       = /* implementation defined */;
static constexpr openmode trunc     = /* implementation defined */;
static constexpr openmode ate       = /* implementation defined */;
static constexpr openmode noreplace = /* implementation defined */;  // (since C++23)
```

Specifies available file open flags. It is a BitmaskType, the following
constants are defined:

- ****`app`**** — seek to the end of stream before each write
- ****`binary`**** — open in binary mode
- ****`in`**** — open for reading
- ****`out`**** — open for writing
- ****`trunc`**** — discard the contents of the stream when opening
- ****`ate`**** — seek to the end of stream immediately after open
- ****`noreplace`** (C++23)** — open in exclusive mode

### Example

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main()
{
    const char* fname = "unique_name.txt";

    // write to a temporary stream object
    std::fstream(fname, std::ios::out | std::ios::trunc) << "Hi";

    std::string s;
    std::fstream(fname, std::ios::in) >> s;
    std::cout << s << '\n';
}
```

Output:

```text
Hi
```

### See also

- **open** — opens a file and configures it as the associated character sequence
  (public member function of `std::basic_filebuf<CharT,Traits>`)
- **(constructor)** — constructs a `basic_stringbuf` object (public member
  function of `std::basic_stringbuf<CharT,Traits,Allocator>`)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/openmode*
