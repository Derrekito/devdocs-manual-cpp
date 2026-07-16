# std::wcsrtombs

```cpp
std::size_t wcsrtombs( char* dst,
                       const wchar_t** src,
                       std::size_t len,
                       std::mbstate_t* ps );
```

Converts a sequence of wide characters from the array whose first element is
pointed to by `*src` to its narrow multibyte representation that begins in the
conversion state described by `*ps`. If `dst` is not null, converted characters
are stored in the successive elements of the char array pointed to by `dst`. No
more than `len` bytes are written to the destination array.

Each character is converted as if by a call to `std::wcrtomb`. The conversion
stops if:

- The null character was converted and stored. `src` is set to a null pointer
  and `*ps` represents the initial shift state.
- A `wchar_t` was found that does not correspond to a valid character in the
  current C locale. `src` is set to point at the first unconverted wide
  character.
- The next multibyte character to be stored would exceed `len`. `src` is set to
  point at the first unconverted wide character. This condition is not checked
  if `dst` is a null pointer.

### Parameters

- **dst** — pointer to narrow character array where the multibyte characters
  will be stored
- **src** — pointer to pointer to the first element of a null-terminated wide
  string
- **len** — number of bytes available in the array pointed to by dst
- **ps** — pointer to the conversion state object

### Return value

On success, returns the number of bytes (including any shift sequences, but
excluding the terminating `'\0'`) written to the character array whose first
element is pointed to by `dst`. If `dst` is a null pointer, returns the number
of bytes that would have been written (again, excluding the terminating null
character `'\0'`).

On conversion error (if invalid wide character was encountered), returns
`static_cast<std::size_t>(-1)`, stores `EILSEQ` in `errno`, and leaves `*ps` in
unspecified state.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <string>
#include <vector>

void print_wide(const wchar_t* wstr)
{
    std::mbstate_t state = std::mbstate_t();
    std::size_t len = 1 + std::wcsrtombs(nullptr, &wstr, 0, &state);
    std::vector<char> mbstr(len);
    std::wcsrtombs(&mbstr[0], &wstr, mbstr.size(), &state);
    std::cout << "multibyte string: " << &mbstr[0] << '\n'
              << "Length, including '\\0': " << mbstr.size() << '\n';
}

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    // UTF-8 narrow multibyte encoding
    const wchar_t* wstr = L"z\u00df\u6c34\U0001d10b"; // or L"zß水𝄋"
    print_wide(wstr);
}
```

Output:

```text
multibyte string: zß水𝄋
Length, including '\0': 11
```

### See also

- **wcrtomb** — converts a wide character to its multibyte representation, given
  state (function)
- **mbsrtowcs** — converts a narrow multibyte character string to wide string,
  given state (function)
- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

**C documentation for `wcsrtombs`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/wcsrtombs*
