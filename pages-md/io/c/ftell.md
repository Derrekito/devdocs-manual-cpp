# std::ftell

```cpp
long ftell( std::FILE* stream );
```

Returns the current value of the file position indicator for the file stream
`stream`.

If the stream is open in binary mode, the value obtained by this function is the
number of bytes from the beginning of the file.

If the stream is open in text mode, the value returned by this function is
unspecified and is only meaningful as the input to `std::fseek`.

### Parameters

- **stream** — file stream to examine

### Return value

File position indicator on success or `-1L` if failure occurs. Also sets `errno`
on failure.

### Example

Demonstrates `std::ftell()` with error checking. Writes then reads a few
floating-point (FP) values to/from a file.

```cpp
#include <cstdio>
#include <cstdlib>
#include <iostream>

// If the condition is not met then exit the program with error message.
void check(bool condition, const char* func, int line)
{
    if (condition)
        return;
    std::perror(func);
    std::cerr << func << " failed in file " << __FILE__ << " at line # " << line - 1
              << '\n';
    std::exit(EXIT_FAILURE);
}

int main()
{
    // Prepare an array of FP values.
    constexpr int SIZE {5};
    double A[SIZE] = {1.1, 2.2, 3.3, 4.4, 5.5};

    // Write array to a file.
    const char* fname = "/tmp/test.bin";
    FILE* file = std::fopen(fname, "wb");
    check(file != NULL, "fopen()", __LINE__);

    const int write_count = std::fwrite(A, sizeof(double), SIZE, file);
    check(write_count == SIZE, "fwrite()", __LINE__);

    std::fclose(file);

    // Read the FP values into array B.
    double B[SIZE];
    file = std::fopen(fname, "rb");
    check(file != NULL, "fopen()", __LINE__);

    long pos = std::ftell(file); // position indicator at start of file
    check(pos != -1L, "ftell()", __LINE__);
    std::cout << "pos: " << pos << '\n';

    const int read_count = std::fread(B, sizeof(double), 1, file); // read one FP value
    check(read_count == 1, "fread()", __LINE__);

    pos = std::ftell(file); // position indicator after reading one FP value
    check(pos != -1L, "ftell()", __LINE__);
    std::cout << "pos: " << pos << '\n';
    std::cout << "B[0]: " << B[0] << '\n'; // print one FP value

    return EXIT_SUCCESS;
}
```

Possible output:

```text
pos: 0
pos: 8
B[0]: 1.1
```

### See also

- **fgetpos** — gets the file position indicator (function)
- **fseek** — moves the file position indicator to a specific location in a file
  (function)
- **fsetpos** — moves the file position indicator to a specific location in a
  file (function)
- **tellg** — returns the input position indicator (public member function of
  `std::basic_istream<CharT,Traits>`)
- **tellp** — returns the output position indicator (public member function of
  `std::basic_ostream<CharT,Traits>`)

**C documentation for `ftell`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/ftell*
