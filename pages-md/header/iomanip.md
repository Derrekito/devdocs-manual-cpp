# Standard library header <iomanip>

This header is part of the Input/output library.

**Functions**

- **resetiosflags** — clears the specified ios_base flags (function)
- **setiosflags** — sets the specified `ios_base` flags (function)
- **setbase** — changes the base used for integer I/O (function)
- **setfill** — changes the fill character (function template)
- **setprecision** — changes floating-point precision (function)
- **setw** — changes the width of the next input/output field (function)
- **get_money (C++11)** — parses a monetary value (function template)
- **put_money (C++11)** — formats and outputs a monetary value (function
  template)
- **get_time (C++11)** — parses a date/time value of specified format (function
  template)
- **put_time (C++11)** — formats and outputs a date/time value according to the
  specified format (function template)
- **quoted (C++14)** — inserts and extracts quoted strings with embedded spaces
  (function template)

### Synopsis

```cpp
namespace std {
  /*unspecified*/ resetiosflags(ios_base::fmtflags mask);
  /*unspecified*/ setiosflags  (ios_base::fmtflags mask);
  /*unspecified*/ setbase(int base);
  template<class CharT> /*unspecified*/ setfill(CharT c);
  /*unspecified*/ setprecision(int n);
  /*unspecified*/ setw(int n);
  template<class MoneyT> /*unspecified*/ get_money(MoneyT& mon, bool intl = false);
  template<class MoneyT> /*unspecified*/ put_money(const MoneyT& mon, bool intl = false);
  template<class CharT> /*unspecified*/ get_time(tm* tmb, const CharT* fmt);
  template<class CharT> /*unspecified*/ put_time(const tm* tmb, const CharT* fmt);

  template<class CharT>
    /*unspecified*/ quoted(const CharT* s, CharT delim = CharT('"'),
                           CharT escape = CharT('\\'));

  template<class CharT, class Traits, class Allocator>
    /*unspecified*/ quoted(const basic_string<CharT, Traits, Allocator>& s,
               CharT delim = CharT('"'), CharT escape = CharT('\\'));

  template<class CharT, class Traits, class Allocator>
    /*unspecified*/ quoted(basic_string<CharT, Traits, Allocator>& s,
               CharT delim = CharT('"'), CharT escape = CharT('\\'));

  template<class CharT, class Traits>
    /*unspecified*/ quoted(basic_string_view<CharT, Traits> s,
               CharT delim = CharT('"'), CharT escape = CharT('\\'));
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/iomanip*
