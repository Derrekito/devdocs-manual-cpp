# std::ends

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>& ends( std::basic_ostream<CharT, Traits>& os );
```

Inserts a null character into the output sequence `os` as if by calling
`os.put(CharT())`.

This is an output-only I/O manipulator, it may be called with an expression such
as `out << std::ends` for any `out` of type `std::basic_ostream`.

### Notes

This manipulator is typically used with `std::ostrstream`, when the associated
output buffer needs to be null-terminated to be processed as a C string.

Unlike `std::endl`, this manipulator does not flush the stream.

### Parameters

- **os** — reference to output stream

### Return value

`os` (reference to the stream after insertion of the null character).

### Example

```cpp
#include <cstdio>
#include <strstream>

int main()
{
    std::ostrstream oss;
    oss << "Sample text: " << 42 << std::ends;
    std::printf("%s\n", oss.str());
    oss.freeze(false); // enable memory deallocation
}
```

Output:

```text
Sample text: 42
```

### See also

- **ostrstream (deprecated in C++98)** — implements character array output
  operations (class)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/ends*
