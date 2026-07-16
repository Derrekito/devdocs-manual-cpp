# std::chrono::operator<<(std::chrono::year_month_day)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::year_month_day& ymd );  // (since C++20)
```

Outputs a textual representation of `ymd` into the stream `os`. This first forms
a `std::basic_string<CharT> s` consisting of a textual representation of the
date in the format `yyyy-mm-dd` (same as the one output by `formatter` with the
%F specifier). Then, if `!ymd.ok()`, appends `" is not a valid date"` to `s`.
Inserts `s` into `os`.

Equivalent to

`return os << (ymd.ok() ? std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{:%F}"), ymd) : std::format(os.getloc(),
STATICALLY_WIDEN<CharT>("{:%F} is not a valid date"), ymd));`

where `STATICALLY_WIDEN<CharT>("...")` is `"..."` if `CharT` is `char`, and
`L"..."` if `CharT` is `wchar_t`.

### Return value

`os`

### Example

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **std::formatter<std::chrono::year_month_day> (C++20)** — formatting support
  for `year_month_day` (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/year_month_day/operator_ltlt*
