# std::basic_istream<CharT,Traits>::getline

```cpp
basic_istream& getline( char_type* s, std::streamsize count );  // (1)
basic_istream& getline( char_type* s, std::streamsize count, char_type delim );  // (2)
```

Extracts characters from stream until end of line or the specified delimiter
`delim`.

The first overload is equivalent to `getline(s, count, widen('\n'))`.

Behaves as UnformattedInputFunction. After constructing and checking the sentry
object, extracts characters from `*this` and stores them in successive locations
of the array whose first element is pointed to by `s`, until any of the
following occurs (tested in the order shown):

1. end of file condition occurs in the input sequence.
1. the next available character `c` is the delimiter, as determined by
   `Traits::eq(c, delim)`. The delimiter is extracted (unlike
   `basic_istream::get()`) and counted towards `gcount()`, but is not stored.
1. `count` is non-positive, or `count - 1` characters have been extracted
   (`setstate(failbit)` is called in this case).

If the function extracts no characters, ​`failbit` is set in the local error
state before `setstate()` is called.

In any case, if `count > 0`, it then stores a null character `CharT()` into the
next successive location of the array and updates `gcount()`.

### Notes

Because condition #2 is tested before condition #3, the input line that exactly
fits the buffer does not trigger `failbit`.

Because the terminating character is counted as an extracted character, an empty
input line does not trigger `failbit`.

### Parameters

- **s** — pointer to the character string to store the characters to
- **count** — size of character string pointed to by `s`
- **delim** — delimiting character to stop the extraction at. It is extracted
  but not stored.

### Return value

`*this`

### Exceptions

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Example

```cpp
#include <array>
#include <iostream>
#include <sstream>
#include <vector>

int main()
{
    std::istringstream input("abc|def|gh");
    std::vector<std::array<char, 4>> v;

    // note: the following loop terminates when std::ios_base::operator bool()
    // on the stream returned from getline() returns false
    for (std::array<char, 4> a; input.getline(&a[0], 4, '|');)
        v.push_back(a);

    for (auto& a : v)
        std::cout << &a[0] << '\n';
}
```

Output:

```text
abc
def
gh
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 531 | C++98 | `std::getline` could not handle the case where `count` is
      non-positive | no character is extracted in this case

### See also

- **getline** — read data from an I/O stream into a string (function template)
- **operator>>** — extracts formatted data (public member function)
- **get** — extracts characters (public member function)
- **read** — extracts blocks of characters (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_istream/getline*
