# std::use_facet

```cpp
template< class Facet >
const Facet& use_facet( const std::locale& loc );
```

Obtains a reference to a facet implemented by `loc`.

The program is ill-formed if Facet is not a facet whose definition contains the
public static member `id` or it is a volatile-qualified facet.

### Parameters

- **loc** — the locale object to query

### Return value

Returns a reference to the facet. The reference returned by this function is
valid as long as any `std::locale` object refers to that facet.

### Exceptions

`std::bad_cast` if `std::has_facet<Facet>(loc) == false`.

### Example

Display the 3-letter currency name used by the user's preferred locale.

```cpp
#include <iostream>
#include <locale>

int main()
{
    for (const char* name: {"en_US.UTF-8", "de_DE.UTF-8", "en_GB.UTF-8"})
    {
        std::locale loc = std::locale(name); // user's preferred locale
        std::cout << "Your currency string is "
                  << std::use_facet<std::moneypunct<char, true>>(loc).
                     curr_symbol() << '\n';
    }
}
```

Output:

```text
Your currency string is USD
Your currency string is EUR
Your currency string is GBP
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 31 | C++98 | the returned reference remained usable as long as the locale
      value itself exists | the returned reference remains usable as long as
      some locale object refers to that facet
  LWG 38 | C++98 | `Facet` was not required to have a direct member `id` |
      required
  LWG 436 | C++98 | it was unclear whether `Facet` can be cv-qualified | it can
      be const-qualified, but not volatile-qualified

### See also

- **locale** — set of polymorphic facets that encapsulate cultural differences
  (class)
- **has_facet** — checks if a locale implements a specific facet (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/locale/use_facet*
