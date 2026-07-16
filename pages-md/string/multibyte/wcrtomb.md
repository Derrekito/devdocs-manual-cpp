# std::wcrtomb

```cpp
std::size_t wcrtomb( char* s, wchar_t wc, std::mbstate_t* ps );
```

Converts a wide character to its narrow multibyte representation.

If `s` is not a null pointer, the function determines the number of bytes
necessary to store the multibyte character representation of `wc` (including any
shift sequences, and taking into account the current multibyte conversion state
`*ps`), and stores the multibyte character representation in the character array
whose first element is pointed to by `s`, updating `*ps` as necessary. At most
`MB_CUR_MAX` bytes can be written by this function.

If `s` is a null pointer, the call is equivalent to `std::wcrtomb(buf, L'\0',
ps)` for some internal buffer `buf`.

If wc is the null wide character `L'\0'`, a null byte is stored, preceded by any
shift sequence necessary to restore the initial shift state and the conversion
state parameter `*ps` is updated to represent the initial shift state.

### Parameters

- **s** — pointer to narrow character array where the multibyte character will
  be stored
- **wc** — the wide character to convert
- **ps** — pointer to the conversion state object used when interpreting the
  multibyte string

### Return value

On success, returns the number of bytes (including any shift sequences) written
to the character array whose first element is pointed to by `s`.

On failure (if `wc` is not a valid wide character), returns
`static_cast<std::size_t>(-1)`, stores `EILSEQ` in `errno`, and leaves `*ps` in
unspecified state.

### Example

```cpp
#include <clocale>
#include <cwchar>
#include <iostream>
#include <string>

void print_wide(const std::wstring& wstr)
{
    std::mbstate_t state{};
    for (wchar_t wc : wstr)
    {
        std::string mb(MB_CUR_MAX, '\0');
        std::size_t ret = std::wcrtomb(&mb[0], wc, &state);
        std::cout << "multibyte char " << mb << " is " << ret << " bytes\n";
    }
}

int main()
{
    std::setlocale(LC_ALL, "en_US.utf8");
    std::wstring wstr = L"z\u00df\u6c34\U0001f34c"; // or L"zß水🍌"
    print_wide(wstr);
}
```

Output:

```text
multibyte char z is 1 bytes
multibyte char ß is 2 bytes
multibyte char 水 is 3 bytes
multibyte char 🍌 is 4 bytes
```

### See also

- **wctomb** — converts a wide character to its multibyte representation
  (function)
- **mbrtowc** — converts the next multibyte character to wide character, given
  state (function)
- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

**C documentation for `wcrtomb`**

---
*Source: https://en.cppreference.com/w/cpp/string/multibyte/wcrtomb*
