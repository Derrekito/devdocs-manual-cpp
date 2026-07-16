# std::strcpy

```cpp
char* strcpy( char* dest, const char* src );
```

Copies the character string pointed to by `src`, including the null terminator,
to the character array whose first element is pointed to by `dest`.

The behavior is undefined if the `dest` array is not large enough. The behavior
is undefined if the strings overlap.

### Parameters

- **dest** — pointer to the character array to write to
- **src** — pointer to the null-terminated byte string to copy from

### Return value

`dest`

### Example

```cpp
#include <cstring>
#include <iostream>
#include <memory>

int main()
{
    const char* src = "Take the test.";
//  src[0] = 'M'; // can't modify string literal
    auto dst = std::make_unique<char[]>(std::strlen(src) + 1); // +1 for null terminator
    std::strcpy(dst.get(), src);
    dst[0] = 'M';
    std::cout << src << '\n' << dst.get() << '\n';
}
```

Output:

```text
Take the test.
Make the test.
```

### See also

- **strncpy** — copies a certain amount of characters from one string to another
  (function)
- **memcpy** — copies one buffer to another (function)

**C documentation for `strcpy`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strcpy*
