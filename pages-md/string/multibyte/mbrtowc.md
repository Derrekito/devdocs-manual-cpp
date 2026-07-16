# std::mbrtowc

```cpp
std::size_t mbrtowc( wchar_t* pwc,
                     const char* s,
                     std::size_t n,
                     std::mbstate_t* ps );
```

Converts a narrow multibyte character to a wide character.

If `s` is not a null pointer, inspects at most `n` bytes of the multibyte
character string, beginning with the byte pointed to by `s` to determine the
number of bytes necessary to complete the next multibyte character (including
any shift sequences). If the function determines that the next multibyte
character in `s` is complete and valid, converts it to the corresponding wide
character and stores it in `*pwc` (if `pwc` is not null).

If `s` is a null pointer, the values of `n` and `pwc` are ignored and call is
equivalent to `std::mbrtowc(nullptr, "", 1, ps)`.

If the wide character produced is the null character, the conversion state
stored in `*ps` is the initial shift state.

### Parameters

- **pwc** — pointer to the location where the resulting wide character will be
  written
- **s** — pointer to the multibyte character string used as input
- **n** — limit on the number of bytes in s that can be examined
- **ps** — pointer to the conversion state used when interpreting the multibyte
  string

### Return value

The first of the following that applies:

- `​0​` if the character converted from `s` (and stored in `pwc` if non-null)
  was the null character.
- the number of bytes `[1...n]` of the multibyte character successfully
  converted from `s`.
- `static_cast<std::size_t>(-2)` if the next `n` bytes constitute an incomplete,
  but so far valid, multibyte character. Nothing is written to `*pwc`.
- `static_cast<std::size_t>(-1)` if encoding error occurs. Nothing is written to
  `*pwc`, the value `EILSEQ` is stored in `errno` and the value of `*ps` is left
  unspecified.

### Example

```cpp
#include <clocale>
#include <cstring>
#include <cwchar>
#include <iostream>

void print_mb(const char* ptr)
{
    std::mbstate_t state = std::mbstate_t(); // initial state
    const char* end = ptr + std::strlen(ptr);
    int len;
    wchar_t wc;
    while ((len = std::mbrtowc(&wc, ptr, end-ptr, &state)) > 0)
    {
        std::wcout << "Next " << len << " bytes are the character " << wc << '\n';
        ptr += len;
    }
}

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    // UTF-8 narrow multibyte encoding
    const char* str = "z\u00df\u6c34\U0001d10b"; // or u8"zß水𝄋"
                      // or "\x7a\xc3\x9f\xe6\xb0\xb4\xf0\x9d\x84\x8b";
    print_mb(str);
}
```

Output:

```text
Next 1 bytes are the character z
Next 2 bytes are the character ß
Next 3 bytes are the character 水
Next 4 bytes are the character 𝄋
```

### See also

- **mbtowc** — converts the next multibyte character to wide character
  (function)
- **wcrtomb** — converts a wide character to its multibyte representation, given
  state (function)
- **do_in [virtual]** — converts a string from `ExternT` to `InternT`, such as
  when reading from file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

**C documentation for `mbrtowc`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/mbrtowc*
