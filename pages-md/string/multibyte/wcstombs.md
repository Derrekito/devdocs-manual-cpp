# std::wcstombs

```cpp
std::size_t wcstombs( char* dst, const wchar_t* src, std::size_t len );
```

Converts a sequence of wide characters from the array whose first element is
pointed to by `src` to its narrow multibyte representation that begins in the
initial shift state. Converted characters are stored in the successive elements
of the char array pointed to by `dst`. No more than `len` bytes are written to
the destination array.

Each character is converted as if by a call to `std::wctomb`, except that the
wctomb's conversion state is unaffected. The conversion stops if:

- The null character was converted and stored.
- A `wchar_t` was found that does not correspond to a valid character in the
  current C locale.
- The next multibyte character to be stored would exceed `len`.

### Notes

In most implementations, this function updates a global static object of type
`std::mbstate_t` as it processes through the string, and cannot be called
simultaneously by two threads, `std::wcsrtombs` should be used in such cases.

POSIX specifies a common extension: if `dst` is a null pointer, this function
returns the number of bytes that would be written to `dst`, if converted.
Similar behavior is standard for `std::wcsrtombs`.

### Parameters

- **dst** — pointer to narrow character array where the multibyte character will
  be stored
- **src** — pointer to the first element of a null-terminated wide string to
  convert
- **len** — number of byte available in the array pointed to by dst

### Return value

On success, returns the number of bytes (including any shift sequences, but
excluding the terminating `'\0'`) written to the character array whose first
element is pointed to by `dst`.

On conversion error (if invalid wide character was encountered), returns
`static_cast<std::size_t>(-1)`.

### Example

```cpp
#include <clocale>
#include <cstdlib>
#include <iostream>

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    // UTF-8 narrow multibyte encoding
    const wchar_t* wstr = L"z\u00df\u6c34\U0001d10b"; // or L"zß水𝄋"
    char mbstr[11];
    std::wcstombs(mbstr, wstr, 11);
    std::cout << "multibyte string: " << mbstr << '\n';
}
```

Output:

```text
multibyte string: zß水𝄋
```

### See also

- **wcsrtombs** — converts a wide string to narrow multibyte character string,
  given state (function)
- **mbstowcs** — converts a narrow multibyte character string to wide string
  (function)
- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

**C documentation for `wcstombs`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/wcstombs*
