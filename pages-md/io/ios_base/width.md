# std::ios_base::width

```cpp
streamsize width() const;  // (1)
streamsize width( streamsize new_width );  // (2)
```

Manages the minimum number of characters to generate on certain output
operations and the maximum number of characters to generate on certain input
operations.

1) Returns the current field width.

2) Sets the field width to the given one. Returns the previous field width.

### Parameters

- **new_width** — new field width setting

### Return value

The field width before the call to the function.

### Notes

Some I/O functions call `width(0)` before returning, see `std::setw` (this
results in this field having effect on the next I/O function only, and not on
any subsequent I/O).

The exact effects this modifier has on the input and output vary between the
individual I/O functions and are described at each `operator<<` and `operator>>`
overload page individually.

### Example

```cpp
#include <algorithm>
#include <array>
#include <chrono>
#include <iomanip>
#include <iostream>
#include <sstream>
#include <string>

std::string to_string(std::chrono::year_month_day ymd)
{
    return (std::ostringstream{} << ymd).str();
}

int main()
{
    using namespace std::literals;

    constexpr int column_size{4};
    using table_t = std::array<std::string, column_size>;

    const auto data = std::to_array<table_t>
    ({
        {"Language", "Author", "Birthdate", "Death date"}, // header
        {"C", "Dennis Ritchie", to_string(1941y/9/9d), to_string(2011y/10/12d)},
        {"C++", "Bjarne Stroustrup", to_string(1950y/12/30d), {}},
        {"C#", "Anders Hejlsberg", to_string(1960y/12/2d), {}},
        {"Python", "Guido van Rossum", to_string(1956y/1/31d), {}},
        {"Javascript", "Brendan Eich", to_string(1961y/7/4d), {}}
    });

    const auto cols_w = [&data] // calculate width of each column
    {
        std::array<int, column_size> width;
        width.fill(0);
        for (auto const& row : data)
            for (auto i{0UZ}; i != row.size(); ++i)
                if (width[i] < static_cast<int>(row[i].size()) + 2)
                    width[i] = static_cast<int>(row[i].size()) + 2;
        return width;
    }(); // IILE

    auto print_line = [&cols_w](table_t const& tbl)
    {
        std::cout << '|';
        for (auto i{0UZ}; auto const& str : tbl)
        {
            std::cout.width(cols_w[i++]);
            std::cout << (' ' + str) << '|';
        }
        std::cout << '\n';
    };

    auto print_break = [&cols_w]
    {
        std::cout.put('+').fill('-');
        std::ranges::for_each(cols_w, [](int w)
        {
            std::cout.width(w);
            std::cout << '-' << '+';
        });
        std::cout.put('\n').fill(' ');
    };

    std::cout.setf(std::ios::left, std::ios::adjustfield);
    print_break();
    for (bool header{true}; auto const& entry : data)
        if (print_line(entry); header)
        {
            print_break();
            header = false;
        }
    print_break();
}
```

Output:

```text
+------------+-------------------+------------+------------+
| Language   | Author            | Birthdate  | Death date |
+------------+-------------------+------------+------------+
| C          | Dennis Ritchie    | 1941-09-09 | 2011-10-12 |
| C++        | Bjarne Stroustrup | 1950-12-30 |            |
| C#         | Anders Hejlsberg  | 1960-12-02 |            |
| Python     | Guido van Rossum  | 1956-01-31 |            |
| Javascript | Brendan Eich      | 1961-07-04 |            |
+------------+-------------------+------------+------------+
```

### See also

- **precision** — manages decimal precision of floating point operations (public
  member function)
- **setw** — changes the width of the next input/output field (function)

---
*Source: https://en.cppreference.com/w/cpp/io/ios_base/width*
