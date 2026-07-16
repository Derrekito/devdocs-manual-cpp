# std::basic_ostream<CharT,Traits>::write

```cpp
basic_ostream& write( const char_type* s, std::streamsize count );
```

Behaves as an UnformattedOutputFunction. After constructing and checking the
sentry object, outputs the characters from successive locations in the character
array whose first element is pointed to by `s`. Characters are inserted into the
output sequence until one of the following occurs:

- exactly `count` characters are inserted
- inserting into the output sequence fails (in which case `setstate(badbit)` is
  called).

### Parameters

- **s** — pointer to the character string to write
- **count** — number of characters to write

### Return value

`*this`

### Exceptions

`failure` if an error occurred (the error state flag is not `goodbit`) and
`exceptions()` is set to throw for that state.

If an internal operation throws an exception, it is caught and `badbit` is set.
If `exceptions()` is set for `badbit`, the exception is rethrown.

### Notes

This function is not overloaded for the types `signed char` or `unsigned char`,
unlike the formatted operator<<.

Also, unlike the formatted output functions, this function does not set the
`failbit` on failure.

When using a non-converting locale (the default locale is non-converting), the
overrider of this function in `std::basic_ofstream` may be optimized for
zero-copy bulk I/O (by means of overriding `std::streambuf::xsputn`).

### Example

This function may be used to output object representations, i.e. binary output

```cpp
#include <iostream>

int main()
{
    int n = 0x41424344;
    std::cout.write(reinterpret_cast<char*>(&n), sizeof n) << '\n';

    char c[] = "This is sample text.";
    std::cout.write(c, 4).write("!\n", 2);
}
```

Possible output:

```text
DCBA
This!
```

### See also

- **operator<<(std::basic_ostream)** — inserts character data or insert into
  rvalue stream (function template)
- **put** — inserts a character (public member function)

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ostream/write*
