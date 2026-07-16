# std::basic_istream<CharT,Traits>::sync

```cpp
int sync();
```

Synchronizes the input buffer with the associated data source.

Behaves as UnformattedInputFunction, except that `gcount()` is not affected.
After constructing and checking the sentry object,

If `rdbuf()` is a null pointer, returns `-1`.

Otherwise, calls `rdbuf()->pubsync()`. If that function returns `-1`, calls
`setstate(badbit)` and returns `-1`. Otherwise, returns `​0​`.

### Parameters

(none)

### Return value

`​0​` on success, `-1` on failure or if the stream does not support this
operation (is unbuffered).

### Notes

As with `readsome()`, it is implementation-defined whether this function does
anything with library-supplied streams. The intent is typically for the next
read operation to pick up any changes that may have been made to the associated
input sequence after the stream buffer last filled its get area. To achieve
that, `sync()` may empty the get area, or it may refill it, or it may do
nothing. A notable exception is Visual Studio, where this operation discards the
unprocessed input when called with a standard input stream.

### Example

Demonstrates the use of input stream `sync()` with file input. Note that output
here is implementation-defined, since calls to `std::basic_filebuf::sync` are
implementation-defined for reads.

```cpp
#include <fstream>
#include <iostream>

void file_abc()
{
    std::ofstream f("test.txt");
    f << "abc\n";
}

void file_123()
{
    std::ofstream f("test.txt");
    f << "123\n";
}

int main()
{
    file_abc(); // file now contains "abc"
    std::ifstream f("test.txt");
    std::cout << "Reading from the file\n";
    char c;
    f >> c;
    std::cout << c;
    file_123(); // file now contains "123"
    f >> c;
    std::cout << c;
    f >> c;
    std::cout << c << '\n';
    f.close();

    file_abc(); // file now contains "abc"
    f.open("test.txt");
    std::cout << "Reading from the file, with sync()\n";
    f >> c;
    std::cout << c;
    file_123(); // file now contains "123"
    f.sync();
    f >> c;
    std::cout << c;
    f >> c;
    std::cout << c << '\n';
}
```

Possible output:

```text
Reading from the file
abc
Reading from the file, with sync()
a23
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 62 | C++98 | `sync()` returned `traits::eof()` if `rdbuf()->pubsync()`
      returns `-1` | returns `-1` in this case

### See also

- **sync [virtual]** — synchronizes the buffers with the associated character
  sequence (virtual protected member function of
  `std::basic_streambuf<CharT,Traits>`)
- **flush** — synchronizes with the underlying storage device (public member
  function of `std::basic_ostream<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/sync*
