# std::strncmp

```cpp
int strncmp( const char* lhs, const char* rhs, std::size_t count );
```

Compares at most `count` characters of two possibly null-terminated arrays. The
comparison is done lexicographically. Characters following the null character
are not compared.

The sign of the result is the sign of the difference between the values of the
first pair of characters (both interpreted as `unsigned char`) that differ in
the arrays being compared.

The behavior is undefined when access occurs past the end of either array `lhs`
or `rhs`. The behavior is undefined when either `lhs` or `rhs` is the null
pointer.

### Parameters

- **lhs, rhs** — pointers to the possibly null-terminated arrays to compare
- **count** — maximum number of characters to compare

### Return value

Negative value if `lhs` appears before `rhs` in lexicographical order.

Zero if `lhs` and `rhs` compare equal, or if `count` is zero.

Positive value if `lhs` appears after `rhs` in lexicographical order.

### Notes

This function is not locale-sensitive, unlike `std::strcoll` and `std::strxfrm`.

### Example

```cpp
#include <cstring>
#include <iostream>

void demo(const char* lhs, const char* rhs, int sz)
{
    const int rc = std::strncmp(lhs, rhs, sz);
    if (rc < 0)
        std::cout << "First " << sz << " chars of ["
                  << lhs << "] precede [" << rhs << "]\n";
    else if (rc > 0)
        std::cout << "First " << sz << " chars of ["
                  << lhs << "] follow [" << rhs << "]\n";
    else
        std::cout << "First " << sz << " chars of ["
                  << lhs << "] equal [" << rhs << "]\n";
}

int main()
{
    demo("Hello, world!", "Hello, everybody!", 13);
    demo("Hello, everybody!", "Hello, world!", 13);
    demo("Hello, everybody!", "Hello, world!", 7);
    demo("Hello, everybody!" + 12, "Hello, somebody!" + 11, 5);
}
```

Output:

```text
First 13 chars of [Hello, world!] follow [Hello, everybody!]
First 13 chars of [Hello, everybody!] precede [Hello, world!]
First 7 chars of [Hello, everybody!] equal [Hello, world!]
First 5 chars of [body!] equal [body!]
```

### See also

- **strcmp** — compares two strings (function)
- **wcsncmp** — compares a certain amount of characters from two wide strings
  (function)
- **memcmp** — compares two buffers (function)
- **strcoll** — compares two strings in accordance to the current locale
  (function)

**C documentation for `strncmp`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/strncmp*
