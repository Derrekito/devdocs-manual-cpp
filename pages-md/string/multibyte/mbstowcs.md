# std::mbstowcs

```cpp
std::size_t mbstowcs( wchar_t* dst, const char* src, std::size_t len );
```

Converts a multibyte character string from the array whose first element is
pointed to by `src` to its wide character representation. Converted characters
are stored in the successive elements of the array pointed to by `dst`. No more
than `len` wide characters are written to the destination array.

Each character is converted as if by a call to `std::mbtowc`, except that the
mbtowc conversion state is unaffected. The conversion stops if:

- The multibyte null character was converted and stored.
- An invalid (in the current C locale) multibyte character was encountered.
- The next wide character to be stored would exceed `len`.

### Notes

In most implementations, this function updates a global static object of type
`std::mbstate_t` as it processes through the string, and cannot be called
simultaneously by two threads, `std::mbsrtowcs` should be used in such cases.

POSIX specifies a common extension: if `dst` is a null pointer, this function
returns the number of wide characters that would be written to `dst`, if
converted. Similar behavior is standard for `std::mbsrtowcs`.

### Parameters

- **dst** — pointer to wide character array where the wide string will be stored
- **src** — pointer to the first element of a null-terminated multibyte string
  to convert
- **len** — number of wide characters available in the array pointed to by dst

### Return value

On success, returns the number of wide characters, excluding the terminating
`L'\0'`, written to the destination array.

On conversion error (if invalid multibyte character was encountered), returns
`static_cast<std::size_t> (-1)`.

### Example

```cpp
#include <clocale>
#include <cstdlib>
#include <iostream>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    std::wcout.imbue(std::locale("en_US.utf8"));
    const char* mbstr = "z\u00df\u6c34\U0001f34c"; // or u8"zß水🍌"
                        // or "\x7a\xc3\x9f\xe6\xb0\xb4\xf0\x9f\x8d\x8c";
    wchar_t wstr[5];
    std::mbstowcs(wstr, mbstr, 5);
    std::wcout << "wide string: " << wstr << '\n';
}
```

Output:

```text
wide string: zß水🍌
```

### See also

- **mbsrtowcs** — converts a narrow multibyte character string to wide string,
  given state (function)
- **wcstombs** — converts a wide string to narrow multibyte character string
  (function)
- **do_in [virtual]** — converts a string from `ExternT` to `InternT`, such as
  when reading from file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

**C documentation for `mbstowcs`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/mbstowcs*
