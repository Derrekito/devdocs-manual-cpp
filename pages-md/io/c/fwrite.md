# std::fwrite

```cpp
std::size_t fwrite( const void* buffer, std::size_t size, std::size_t count, std::FILE* stream );
```

Writes up to `count` binary objects from the given array `buffer` to the output
stream `stream`. The objects are written as if by reinterpreting each object as
an array of `unsigned char` and calling `std::fputc` `size` times for each
object to write those `unsigned char`s into `stream`, in order. The file
position indicator for the stream is advanced by the number of characters
written.

If the objects are not TriviallyCopyable, the behavior is undefined.

If an error occurs, the resulting value of the file position indicator for the
stream is indeterminate.

### Parameters

- **buffer** — pointer to the first object in the array to be written
- **size** — size of each object
- **count** — the number of the objects to be written
- **stream** — output file stream to write to

### Return value

Number of objects written successfully, which may be less than `count` if an
error occurred.

If `size` or `count` is zero, `fwrite` returns zero and performs no other
action.

### Example

```cpp
#include <array>
#include <cstdio>
#include <vector>

int main ()
{
    // write buffer to file
    if (std::FILE* f1 = std::fopen("file.bin", "wb"))
    {
        std::array<int, 3> v = {42, -1, 7}; // underlying storage of std::array is an array
        std::fwrite(v.data(), sizeof v[0], v.size(), f1);
        std::fclose(f1);
    }

    // read the same data and print it to the standard output
    if (std::FILE* f2 = std::fopen("file.bin", "rb"))
    {
        std::vector<int> rbuf(10); // underlying storage of std::vector is also an array
        std::size_t sz = std::fread(rbuf.data(), sizeof rbuf[0], rbuf.size(), f2);
        std::fclose(f2);
        for (std::size_t n = 0; n < sz; ++n)
            std::printf("%d\n", rbuf[n]);
    }
}
```

Output:

```text
42
-1
7
```

### See also

- **printffprintfsprintfsnprintf (C++11)** — prints formatted output to
  `stdout`, a file stream or a buffer (function)
- **fputs** — writes a character string to a file stream (function)
- **fread** — reads from a file (function)

**C documentation for `fwrite`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/fwrite*
