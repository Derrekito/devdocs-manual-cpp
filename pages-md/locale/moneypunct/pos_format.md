# std::moneypunct<CharT,International>::pos_format, do_pos_format, neg_format, do_neg_format

```cpp
public:
pattern pos_format() const;  // (1)
public:
pattern neg_format() const;  // (2)
protected:
virtual pattern do_pos_format() const;  // (3)
protected:
virtual pattern do_neg_format() const;  // (4)
```

1) Public member function, calls the member function `do_pos_format` of the most
   derived class.

2) Public member function, calls the member function `do_neg_format` of the most
   derived class.

3) Returns the format structure (of type `std::money_base::format`) which
   describes the formatting of positive monetary values.

4) Returns the format structure (of type `std::money_base::format`) which
   describes the formatting of negative monetary values.

The standard specializations of `std::moneypunct` return the pattern `{symbol,
sign, none, value}`.

### Return value

The object of type `std::money_base::format` describing the formatting used by
this locale.

### Notes

While `std::money_put` uses `pos_format` for formatting positive values and
neg_format for formatting negative values, `std::money_get` uses `neg_format`
for parsing all monetary values: it assumes that `neg_format` is compatible with
`pos_format`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <locale>

struct my_punct : std::moneypunct_byname<char, false>
{
    my_punct(const char* name) : moneypunct_byname(name) {}
    pattern do_pos_format() const { return {value, space, symbol, sign}; }
    pattern do_neg_format() const { return {value, space, symbol, sign}; }
};

int main()
{
    std::cout.imbue(std::locale("en_US.utf8"));
    std::cout << "american locale: " << std::showbase
              << std::put_money(12345678.0) << '\n';

    std::cout.imbue(std::locale(std::cout.getloc(), new my_punct("en_US.utf8")));
    std::cout << "locale with modified moneypunct:\n"
              << std::put_money(12345678.0) << '\n'
              << std::put_money(-12345678.0) << '\n';
}
```

Output:

```text
american locale: $123,456.78
locale with modified moneypunct:
123,456.78 $
123,456.78 $-
```

### See also

- **do_curr_symbol [virtual]** — provides the string to use as the currency
  identifier (virtual protected member function)
- **do_positive_signdo_negative_sign [virtual]** — provides the string to
  indicate a positive or negative value (virtual protected member function)
- **do_get [virtual]** — parses a monetary value from an input stream (virtual
  protected member function of `std::money_get<CharT,InputIt>`)
- **do_put [virtual]** — formats a monetary value and writes to output stream
  (virtual protected member function of `std::money_put<CharT,OutputIt>`)

---
*Source: https://en.cppreference.com/w/cpp/locale/moneypunct/pos_format*
