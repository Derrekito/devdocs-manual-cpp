# std::time_base

```cpp
class time_base;
```

The class `std::time_base` provides the date order constants which are inherited
by the `std::time_get` facets.

### Member types

- **`enum dateorder { no_order, dmy, mdy, ymd, ydm };`** — Unscoped enumeration
  type

- **`no_order`** — Unspecified order
- **`dmy`** — Day, month, year (european) order
- **`mdy`** — Month, day, year (american) order
- **`ymd`** — Year, month, day (asian) order
- **`ydm`** — Year, day, month

### See also

- **do_date_order [virtual]** — obtains preferred ordering of day, month, and
  year (virtual protected member function of `std::time_get<CharT,InputIt>`)
- **do_get_date [virtual]** — extracts month, day, and year from input stream
  (virtual protected member function of `std::time_get<CharT,InputIt>`)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_base*
