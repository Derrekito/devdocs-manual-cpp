# std::wcstok

```cpp
wchar_t* wcstok( wchar_t* str, const wchar_t* delim, wchar_t ** ptr);
```

Finds the next token in a null-terminated wide string pointed to by `str`. The
separator characters are identified by null-terminated wide string pointed to by
`delim`.

This function is designed to be called multiples times to obtain successive
   tokens from the same string.

- If `str != nullptr`, the call is treated as the first call to `std::wcstok`
  for this particular wide string. The function searches for the first wide
  character which is *not* contained in `delim`.
- If no such wide character was found, there are no tokens in `str` at all, and
  the function returns a null pointer.
- If such wide character was found, it is the *beginning of the token*. The
  function then searches from that point on for the first wide character that
  *is* contained in `delim`.
- The function then returns the pointer to the beginning of the token.
- If `str == nullptr`, the call is treated as a subsequent calls to
  `std::wcstok`: the function continues from where it left in previous
  invocation with the same `*ptr`. The behavior is the same as if the pointer to
  the wide character that follows the last detected token is passed as `str`.

### Parameters

- **str** — pointer to the null-terminated wide string to tokenize
- **delim** — pointer to the null-terminated wide string identifying delimiters
- **ptr** — pointer to an object of type `wchar_t*`, which is used by wcstok to
  store its internal state

### Return value

Pointer to the beginning of the next token or null pointer if there are no more
tokens.

### Note

This function is destructive: it writes the `L'\0'` characters in the elements
of the string `str`. In particular, a wide string literal cannot be used as the
first argument of `std::wcstok`.

Unlike `std::strtok`, this function does not update static storage: it stores
the parser state in the user-provided location.

Unlike most other tokenizers, the delimiters in `std::wcstok` can be different
for each subsequent token, and can even depend on the contents of the previous
tokens.

### Example

```cpp
#include <cwchar>
#include <iostream>

int main()
{
    wchar_t input[100] = L"A bird came down the walk";
    wchar_t* buffer;
    wchar_t* token = std::wcstok(input, L" ", &buffer);
    while (token)
    {
        std::wcout << token << '\n';
        token = std::wcstok(nullptr, L" ", &buffer);
    }
}
```

Output:

```text
A
bird
came
down
the
walk
```

### See also

- **strtok** — finds the next token in a byte string (function)

**C documentation for `wcstok`**

---
*Source: https://en.cppreference.com/w/cpp/string/wide/wcstok*
