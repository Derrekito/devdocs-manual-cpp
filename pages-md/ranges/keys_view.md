# std::ranges::views::keys, std::ranges::keys_view

```cpp
template< class R >
using keys_view = ranges::elements_view<R, 0>;  // (1) (since C++20)
namespace views {
    inline constexpr auto keys = ranges::elements<0>;
}  // (2) (since C++20)
```

Takes a `view` of *tuple-like* values (e.g. `std::tuple` or `std::pair`), and
produces a view with a *value-type* of the *first* element of the adapted view's
value-type.

1) An alias for `ranges::elements_view<R, 0>`.

2) RangeAdaptorObject (and also RangeAdaptorClosureObject). The expression
   `views::keys(e)` is expression-equivalent to
   `keys_view<views::all_t<decltype((e))>>{e}` for any suitable subexpression
   `e`.

### Notes

`keys_view` can be useful for extracting *keys* from associative containers,
e.g.

```cpp
std::map<std::string, int> map{{"one", 1}, {"two", 2}};

for (auto const& key : std::views::keys(map))
    std::cout << key << ' ';
// prints: one two
```

### Example

Displays values for each type of quark in particle physics.

```cpp
#include <iomanip>
#include <iostream>
#include <locale>
#include <ranges>
#include <string>
#include <tuple>
#include <vector>

int main()
{
    const std::vector<std::tuple<std::string, double, bool>> quark_mass_charge
    {
        // name, MeV/c², has positive electric-charge:
        {"up", 2.3, true}, {"down", 4.8, false},
        {"charm", 1275, true}, {"strange", 95, false},
        {"top", 173'210, true}, {"bottom", 4'180, false},
    };

    std::cout.imbue(std::locale("en_US.utf8"));
    std::cout << "Quark name:  │ ";
    for (std::string const& name : std::views::keys(quark_mass_charge))
        std::cout << std::setw(9) << name << " │ ";

    std::cout << "\n" "Mass MeV/c²: │ ";
    for (const double mass : std::views::values(quark_mass_charge))
        std::cout << std::setw(9) << mass << " │ ";

    std::cout << "\n" "E-charge:    │ ";
    for (const bool pos : std::views::elements<2>(quark_mass_charge))
        std::cout << std::setw(9) << (pos ? "+2/3" : "-1/3") << " │ ";
    std::cout << '\n';
}
```

Output:

```text
Quark name:  │        up │      down │     charm │   strange │       top │    bottom │
Mass MeV/c²: │       2.3 │       4.8 │     1,275 │        95 │   173,210 │     4,180 │
E-charge:    │      +2/3 │      -1/3 │      +2/3 │      -1/3 │      +2/3 │      -1/3 │
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3563 | C++20 | `keys_view` is unable to participate in CTAD due to its use
      of `views::all_t` | `views::all_t` removed

### See also

- **ranges::values_viewviews::values (C++20)** — takes a `view` consisting of
  pair-like values and produces a `view` of the second elements of each pair
  (class template) (range adaptor object)
- **ranges::elements_viewviews::elements (C++20)** — takes a `view` consisting
  of `tuple-like` values and a number N and produces a `view` of Nth element of
  each tuple (class template) (range adaptor object)
- **slice** — BLAS-like slice of a valarray: starting index, length, stride
  (class)

---
*Source: https://en.cppreference.com/w/cpp/ranges/keys_view*
