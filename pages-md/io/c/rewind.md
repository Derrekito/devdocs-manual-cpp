# std::rewind

```cpp
void rewind( std::FILE* stream );
```

Moves the file position indicator to the beginning of the given file stream.

The function is equivalent to `std::fseek(stream, 0, SEEK_SET);`, except that
end-of-file and error indicators are cleared.

The function drops any effects from previous calls to `ungetc`.

### Parameters

- **stream** — file stream to modify

### Return value

(none)

### Example

```cpp
#include <array>
#include <cstdio>

int main()
{
    std::FILE* f = std::fopen("file.txt", "w");
    for (char ch = '0'; ch <= '9'; ch++)
        std::fputc(ch, f);
    std::fclose(f);

    std::array<char, 20> str;
    std::FILE* f2 = std::fopen("file.txt", "r");

    const unsigned size1 = std::fread(str.data(), 1, str.size(), f2);
    std::puts(str.data());
    std::printf("size1 = %u\n", size1);

    std::rewind(f2);

    const unsigned size2 = std::fread(str.data(), 1, str.size(), f2);
    std::puts(str.data());
    std::printf("size2 = %u", size2);

    std::fclose(f2);
}
```

Output:

```text
0123456789
size1 = 10
0123456789
size2 = 10
```

### See also

- **fseek** — moves the file position indicator to a specific location in a file
  (function)

**C documentation for `rewind`**

---
*Source: https://en.cppreference.com/w/cpp/io/c/rewind*
