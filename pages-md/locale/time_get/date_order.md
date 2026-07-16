# std::time_get<CharT,InputIt>::date_order, std::time_get<CharT,InputIt>::do_date_order

```cpp
public:
dateorder date_order() const;  // (1)
protected:
virtual dateorder do_date_order() const;  // (2)
```

1) Public member function, calls the protected virtual member function
   `do_date_order` of the most derived class.

2) Returns a value of type `std::time_base::dateorder`, which describes the
   default date format used by this locale (expected by `get_date()` and
   produced by `std::strftime()` with format specifier `'%x'`).

The valid values (inherited from `std::time_base`) are:

- **`no_order`** — the format contains variable items (week day, Julian day,
  etc), or this function is not implemented
- **`dmy`** — day, month, year (European locales)
- **`mdy`** — month, day, year (American locales)
- **`ymd`** — year, month, day (Asian locales)
- **`ydm`** — year, day, month (rare)

### Parameters

(none)

### Return value

A value of type `dateorder`.

### Notes

This function is optional, it may return `no_order` in every case.

### Example

The output below was obtained using clang (libc++).

```cpp
#include <iostream>
#include <locale>

void show_date_order()
{
    std::time_base::dateorder d = std::use_facet<std::time_get<char>>(
                                      std::locale()
                                  ).date_order();
    switch (d)
    {
        case std::time_base::no_order:
            std::cout << "no_order\n";
            break;
        case std::time_base::dmy:
            std::cout << "day, month, year\n";
            break;
        case std::time_base::mdy:
            std::cout << "month, day, year\n";
            break;
        case std::time_base::ymd:
            std::cout << "year, month, day\n";
            break;
        case std::time_base::ydm:
            std::cout << "year, day, month\n";
            break;
    }
}

int main()
{
    std::locale::global(std::locale("en_US.utf8"));
    std::cout << "In U.S. locale, the default date order is: ";
    show_date_order();

    std::locale::global(std::locale("ja_JP.utf8"));
    std::cout << "In Japanese locale, the default date order is: ";
    show_date_order();

    std::locale::global(std::locale("de_DE.utf8"));
    std::cout << "In German locale, the default date order is: ";
    show_date_order();
}
```

Possible output:

```text
In U.S. locale, the default date order is: month, day, year
In Japanese locale, the default date order is: year, month, day
In German locale, the default date order is: day, month, year
```

### See also

- **do_get_date [virtual]** — extracts month, day, and year from input stream
  (virtual protected member function)
- **time_base** — defines date format constants (class)

---
*Source: https://en.cppreference.com/w/cpp/locale/time_get/date_order*
