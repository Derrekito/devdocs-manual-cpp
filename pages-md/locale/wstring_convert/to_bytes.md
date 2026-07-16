# std::wstring_convert<Codecvt,Elem,Wide_alloc,Byte_alloc>::to_bytes

```cpp
byte_string to_bytes( Elem wchar );  // (1)
byte_string to_bytes( const Elem* wptr );  // (2)
byte_string to_bytes( const wide_string& wstr );  // (3)
byte_string to_bytes( const Elem* first, const Elem* last );  // (4)
```

Performs wide to multibyte conversion, using the `codecvt` facet supplied at
construction.

1) Converts `wchar` as if it was a string of length `1`, to `byte_string`.

2) Converts the null-terminated wide character sequence beginning at the wide
   character pointed to by `wptr`, to `byte_string`.

3) Converts the wide string `wstr` to `byte_string`.

4) Converts the wide character sequence `[``first``,``last``)` to `byte_string`.

In all cases, the conversion begins in initial shift state, unless non-initial
starting state was provided to this `wstring_convert` constructor. The number of
characters converted and the final value of the conversion state are remembered
and can be accessed with `state()` and `converted()`.

### Return value

A `byte_string` object containing the results of the wide to multibyte
conversion. If the conversion failed and there was a user-supplied byte-error
string provided to the constructor of this `wstring_convert`, returns that
byte-error string.

### Exceptions

If this `wstring_convert` object was constructed without a user-supplied
byte-error string, throws `std::range_error` on conversion failure.

### Example

```cpp
#include <codecvt>
#include <iomanip>
#include <iostream>
#include <locale>
#include <string>

// utility function for output
void hex_print(const std::string& s)
{
    std::cout << std::hex << std::setfill('0');
    for (unsigned char c : s)
        std::cout << std::setw(2) << static_cast<int>(c) << ' ';
    std::cout << std::dec << '\n';
}

int main()
{
    // wide character data
    std::wstring wstr = L"z\u00df\u6c34\U0001f34c"; // or L"zß水🍌"

    // wide to UTF-8
    std::wstring_convert<std::codecvt_utf8<wchar_t>> conv1;
    std::string u8str = conv1.to_bytes(wstr);
    std::cout << "UTF-8 conversion produced " << u8str.size() << " bytes:\n";
    hex_print(u8str);

    // wide to UTF-16le
    std::wstring_convert<std::codecvt_utf16<wchar_t, 0x10ffff, std::little_endian>> conv2;
    std::string u16str = conv2.to_bytes(wstr);
    std::cout << "UTF-16le conversion produced " << u16str.size() << " bytes:\n";
    hex_print(u16str);
}
```

Output:

```text
UTF-8 conversion produced 10 bytes:
7a c3 9f e6 b0 b4 f0 9f 8d 8c
UTF-16le conversion produced 10 bytes:
7a 00 df 00 34 6c 3c d8 4c df
```

### See also

- **from_bytes** — converts a byte string into a wide string (public member
  function)
- **wcsrtombs** — converts a wide string to narrow multibyte character string,
  given state (function)
- **do_out [virtual]** — converts a string from `InternT` to `ExternT`, such as
  when writing to file (virtual protected member function of
  `std::codecvt<InternT,ExternT,StateT>`)

---
*Source: https://en.cppreference.com/w/cpp/locale/wstring_convert/to_bytes*
