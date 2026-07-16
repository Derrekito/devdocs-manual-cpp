# std::moneypunct<CharT,International>::curr_symbol, do_curr_symbol

```cpp
public:
string_type curr_symbol() const;  // (1)
protected:
virtual string_type do_curr_symbol() const;  // (2)
```

1) Public member function, calls the member function `do_curr_symbol` of the
   most derived class.

2) Returns the string used as the currency identifier by this locale. If
   `International` (the second template parameter of `std::moneypunct`) is
   `false`, the identifier is usually a single (wide) character, such as `"￥"`
   or `"$"`. If `International` is `true`, the identifier is usually a
   four-character string holding the three-character ISO 4217 currency code
   followed by a space (`"JPY "` or `"USD "`).

### Return value

The object of type `string_type` holding the currency symbol or code.

### Example

```cpp
#include <iostream>
#include <locale>

void show_ccy(const char* locname)
{
    std::locale loc(locname);
    std::cout << locname << " currency symbol is "
              << std::use_facet<std::moneypunct<char, true>>(loc).curr_symbol()
              << "or " << std::use_facet<std::moneypunct<char>>(loc).curr_symbol()
              << " for short\n";
}

int main()
{
    show_ccy("en_US.utf8");
    show_ccy("ja_JP.utf8");
    show_ccy("sv_SE.utf8");
    show_ccy("ru_RU.utf8");
    show_ccy("vi_VN.utf8");
}
```

Output:

```text
en_US.utf8 currency symbol is USD or $ for short
ja_JP.utf8 currency symbol is JPY or ￥ for short
sv_SE.utf8 currency symbol is SEK or kr for short
ru_RU.utf8 currency symbol is RUB or руб for short
vi_VN.utf8 currency symbol is VND or ₫ for short
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 666 | C++98 | the length of the identifier string was required to be `4`
      if `International` is `true` | not required

### See also

- **do_pos_formatdo_neg_format [virtual]** — provides the formatting pattern for
  currency values (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/moneypunct/curr_symbol*
