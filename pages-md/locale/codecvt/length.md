# std::codecvt<InternT,ExternT,StateT>::length, do_length

```cpp
public:
int length( StateT& state, const ExternT* from, const ExternT* from_end,
            std::size_t max ) const;  // (1)
protected:
virtual int do_length( StateT& state, const ExternT* from, const ExternT* from_end,
                       std::size_t max ) const;  // (2)
```

1) Public member function, calls the member function `do_length` of the most
   derived class.

2) Attempts to convert the `ExternT` characters from the character array defined
   by `[``from``,``from_end``)`, given initial conversion state `state`, to at
   most `max` `InternT` characters, and returns the number of `ExternT`
   characters that such conversion would consume. Modifies `state` as if by
   executing `do_in(state, from, from_end, from, to, to + max, to)` for some
   imaginary `[``to``,``to + max``)` output buffer.

### Return value

The number of `ExternT` characters that would be consumed if converted by
`do_in()` until either all `from_end - from` characters were consumed or `max`
`InternT` characters were produced, or a conversion error occurred.

The non-converting specialization `std::codecvt<char, char, std::mbstate_t>`
returns `std::min(max, from_end - from)`.

### Example

```cpp
#include <iostream>
#include <locale>
#include <string>

int main()
{
    using facet_type = std::codecvt<char16_t, char, std::mbstate_t>;

    // narrow multibyte encoding
    std::string s = "z\u00df\u6c34\U0001d10b"; // or u8"zß水𝄋"
              // or "\x7a\xc3\x9f\xe6\xb0\xb4\xf0\x9d\x84\x8b"

    std::setlocale(LC_ALL, "en_US.UTF-8");
    facet_type const& codecvt_facet = std::use_facet<facet_type>(std::locale());
    std::mbstate_t mb = std::mbstate_t();
    std::cout << "Only the first "
              << codecvt_facet.length(mb, s.data(), s.data() + s.size(), 2)
              << " bytes out of " << s.size() << " would be consumed"
                 " to produce the first 2 characters\n";
}
```

Output:

```text
Only the first 3 bytes out of 10 would be consumed to produce the first 2 characters
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 75 | C++98 | the effect on `state` was not specified | specified
  LWG 305 | C++98 | `std::codecvt<wchar_t, char, std::mbstate_t>::do_length` was
      required to return `std::min(max, from_end - from)` | not required

### See also

- **do_in [virtual]** — converts a string from `ExternT` to `InternT`, such as
  when reading from file (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/codecvt/length*
