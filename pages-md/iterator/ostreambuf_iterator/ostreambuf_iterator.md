# std::ostreambuf_iterator<CharT,Traits>::ostreambuf_iterator

```cpp
ostreambuf_iterator( streambuf_type* buffer ) throw();  // (until C++11)
ostreambuf_iterator( streambuf_type* buffer ) noexcept;  // (since C++11)
ostreambuf_iterator( ostream_type& stream ) throw();  // (until C++11)
ostreambuf_iterator( ostream_type& stream ) noexcept;  // (since C++11)
```

1) Constructs the iterator with the private `streambuf_type*` member set to
   `buffer` and the `failed()` flag set to `false`. The behavior is undefined if
   `buffer` is a null pointer.

2) Same as `ostreambuf_iterator(stream.rdbuf())`.

### Parameters

- **stream** — the output stream whose `rdbuf()` will be accessed by this
  iterator
- **buffer** — the output stream buffer to be accessed by this iterator

### Example

```cpp
#include <fstream>
#include <iostream>
#include <iterator>

int main()
{
    const char* file = "test.txt";
    {
        std::basic_filebuf<char> f;
        f.open(file, std::ios::out);
        std::ostreambuf_iterator<char> out1(&f);
        *out1 = 'a'; // writes to file via iterator
    }

    // read back from the file
    char a;
    std::cout << ((std::ifstream{file} >> a), a) << std::endl;

    std::ostreambuf_iterator<wchar_t> out2{std::wcout};
    *out2 = L'b';
}
```

Output:

```text
a
b
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 112 | C++98 | the requirement "the argument cannot be null" was applied to
      overload (2) | applies to overload (1) instead
  P2325R3 | C++20 | default constructor was provided as C++20 iterators must be
      `default_initializable` | removed along with the requirement

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostreambuf_iterator/ostreambuf_iterator*
