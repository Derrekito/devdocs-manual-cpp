# std::ostrstream::ostrstream

```cpp
ostrstream();  // (1)
ostrstream(char* s, int n, std::ios_base::openmode mode = std::ios_base::out);  // (2)
```

Constructs new output strstream and its underlying `std::strstreambuf`.

1) Default-constructs the underlying `std::strstreambuf`, which creates a
   dynamically growing buffer, and initializes the base class with the address
   of the strstreambuf member.

2) Initialized the base class with the address of the underlying
   `std::strstreambuf` member, which is initialized in one of the two possible
   ways, both of which write to user-provided fixed-size array:

   a) if the `app` bit is not set in `mode`, constructs the buffer by calling
      `strstreambuf(s, n, s)`. The behavior is undefined if there are less than
      `n` elements in the array whose first element is pointed to by `s`

   b) if the `app` bit is set in `mode`, constructs the buffer by calling
      `strstreambuf(s, n, s + std::strlen(s))`. The behavior is undefined if
      there are less than `n` elements in the array whose first element is
      pointed to by `s` or if the array does not contain a valid null-terminated
      character sequence.

### Parameters

- **s** — char array to use as the output buffer
- **n** — size of the array to be used as the output buffer
- **mode** — specifies stream open mode. It is a bitmask type, the following
  constants are defined (although only `app` is used): Constant Explanation
  `app` seek to the end of stream before each write `binary` open in binary mode
  `in` open for reading `out` open for writing `trunc` discard the contents of
  the stream when opening `ate` seek to the end of stream immediately after open
  `noreplace` (C++23) open in exclusive mode
- **`app`** — seek to the end of stream before each write
- **`binary`** — open in binary mode
- **`in`** — open for reading
- **`out`** — open for writing
- **`trunc`** — discard the contents of the stream when opening
- **`ate`** — seek to the end of stream immediately after open
- **`noreplace` (C++23)** — open in exclusive mode

### Example

```cpp
#include <iostream>
#include <strstream>

int main()
{
    std::ostrstream s1; // dynamic buffer
    s1 << 1 << ' ' << 3.14 << " example\n" << std::ends;
    std::cout << s1.str();
    s1.freeze(false);

    char arr[15] = "Hello";

    std::ostrstream s2(arr, sizeof arr, std::ios_base::app);
    s2 << ", world!" << std::ends;
    std::cout << s2.str() << '\n';
    std::cout << arr << '\n'; // streams use the provided arrays
}
```

Output:

```text
1 3.14 example
Hello, world!
Hello, world!
```

### See also

- **(constructor)** — constructs a `strstreambuf` object (public member function
  of `std::strstreambuf`)
- **(constructor)** — constructs an `istrstream` object, optionally allocating
  the buffer (public member function of `std::istrstream`)
- **(constructor)** — constructs a `strstream` object, optionally allocating the
  buffer (public member function of `std::strstream`)

---
*Source: https://en.cppreference.com/w/cpp/io/ostrstream/ostrstream*
