# std::moneypunct<CharT,International>::grouping, do_grouping

```cpp
public:
std::string grouping() const;  // (1)
protected:
virtual std::string do_grouping() const;  // (2)
```

1) Public member function, calls the member function `do_grouping` of the most
   derived class.

2) Returns the pattern that determines the grouping of the digits in the
   monetary output, with the same exact meaning as `std::numpunct::do_grouping`.

### Return value

The object of type `std::string` holding the groups. The standard
specializations of `std::moneypunct` return an empty string, indicating no
grouping. Typical groupings (e.g. the `en_US` locale) return `"\003"`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <iterator>
#include <locale>

struct space_out : std::moneypunct<char>
{
    pattern do_pos_format() const { return {value, none, none, none}; }
    int do_frac_digits() const { return 0; }
    char_type do_thousands_sep() const { return ' '; }
    string_type do_grouping() const { return "\002"; }
};

int main()
{
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "american locale: " << std::showbase
              << std::put_money(12345678.0) << '\n';

    std::cout.imbue(std::locale(std::cout.getloc(), new space_out));
    std::cout << "locale with modified moneypunct: "
              << std::put_money(12345678.0) << '\n';
}
```

Output:

```text
american locale: $123,456.78
locale with modified moneypunct: 12 34 56 78
```

### See also

- **do_thousands_sep [virtual]** — provides the character to use as thousands
  separator (virtual protected member function)
- **do_decimal_point [virtual]** — provides the character to use as decimal
  point (virtual protected member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/moneypunct/grouping*
