# Standard library header <cstring>

This header was originally in the C standard library as `<string.h>`.

This header is for C-style null-terminated byte strings.

### Macros

- **NULL** — implementation-defined null pointer constant (macro constant)

### Types

- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)

### Functions

**String manipulation**

- **strcpy** — copies one string to another (function)
- **strncpy** — copies a certain amount of characters from one string to another
  (function)
- **strcat** — concatenates two strings (function)
- **strncat** — concatenates a certain amount of characters of two strings
  (function)
- **strxfrm** — transform a string so that `strcmp` would produce the same
  result as `strcoll` (function)

**String examination**

- **strlen** — returns the length of a given string (function)
- **strcmp** — compares two strings (function)
- **strncmp** — compares a certain number of characters from two strings
  (function)
- **strcoll** — compares two strings in accordance to the current locale
  (function)
- **strchr** — finds the first occurrence of a character (function)
- **strrchr** — finds the last occurrence of a character (function)
- **strspn** — returns the length of the maximum initial segment that consists
  of only the characters found in another byte string (function)
- **strcspn** — returns the length of the maximum initial segment that consists
  of only the characters not found in another byte string (function)
- **strpbrk** — finds the first location of any character from a set of
  separators (function)
- **strstr** — finds the first occurrence of a substring of characters
  (function)
- **strtok** — finds the next token in a byte string (function)

**Character array manipulation**

- **memchr** — searches an array for the first occurrence of a character
  (function)
- **memcmp** — compares two buffers (function)
- **memset** — fills a buffer with a character (function)
- **memcpy** — copies one buffer to another (function)
- **memmove** — moves one buffer to another (function)

**Miscellaneous**

- **strerror** — returns a text version of a given error code (function)

### Notes

- `NULL` is also defined in the following headers: `<clocale>` `<ctime>`
  `<cstddef>` `<cstdio>` `<cwchar>`
- `std::size_t` is also defined in the following headers: `<ctime>` `<cstddef>`
  `<cstdio>` `<cuchar>` (since C++17) `<cwchar>`

### Synopsis

```cpp
namespace std {
  using size_t = /* see description */;                  // freestanding

  void* memcpy(void* s1, const void* s2, size_t n);      // freestanding
  void* memmove(void* s1, const void* s2, size_t n);     // freestanding
  char* strcpy(char* s1, const char* s2);                // freestanding
  char* strncpy(char* s1, const char* s2, size_t n);     // freestanding
  char* strcat(char* s1, const char* s2);                // freestanding
  char* strncat(char* s1, const char* s2, size_t n);     // freestanding
  int memcmp(const void* s1, const void* s2, size_t n);  // freestanding
  int strcmp(const char* s1, const char* s2);            // freestanding
  int strcoll(const char* s1, const char* s2);
  int strncmp(const char* s1, const char* s2, size_t n); // freestanding
  size_t strxfrm(char* s1, const char* s2, size_t n);
  const void* memchr(const void* s, int c, size_t n);    // freestanding
  void* memchr(void* s, int c, size_t n);                // freestanding
  const char* strchr(const char* s, int c);              // freestanding
  char* strchr(char* s, int c);                          // freestanding
  size_t strcspn(const char* s1, const char* s2);        // freestanding
  const char* strpbrk(const char* s1, const char* s2);   // freestanding
  char* strpbrk(char* s1, const char* s2);               // freestanding
  const char* strrchr(const char* s, int c);             // freestanding
  char* strrchr(char* s, int c);                         // freestanding
  size_t strspn(const char* s1, const char* s2);         // freestanding
  const char* strstr(const char* s1, const char* s2);    // freestanding
  char* strstr(char* s1, const char* s2);                // freestanding
  char* strtok(char* s1, const char* s2);                // freestanding
  void* memset(void* s, int c, size_t n);                // freestanding
  char* strerror(int errnum);
  size_t strlen(const char* s);                          // freestanding
}

#define NULL /* see description */                       // freestanding
```

---
*Source: https://en.cppreference.com/w/cpp/header/cstring*
