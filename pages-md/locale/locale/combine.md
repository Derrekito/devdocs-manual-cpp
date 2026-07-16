# std::locale::combine

```cpp
template< class Facet >
locale combine( const locale& other ) const;
```

Constructs a locale object which is a copy of `*this` except for the facet of
type `Facet`, which is copied from `other`.

The program is ill-formed if Facet is not a facet or it is a volatile-qualified
facet.

### Return value

The new, nameless, locale.

### Exceptions

`std::runtime_error` if `other` does not implement `Facet`.

### Example

```cpp
#include <iostream>
#include <locale>

int main()
{
    const double number = 1000.25;
    std::cout << "\"C\" locale: " << number << '\n';
    std::locale loc = std::locale()
        .combine<std::numpunct<char>>(std::locale("en_US.UTF8"));
    std::cout.imbue(loc);
    std::cout << "\"C\" locale with en_US numpunct: " << number << '\n';
}
```

Output:

```text
"C" locale: 1000.25
"C" locale with en_US numpunct: 1,000.25
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 14 | C++98 | `locale::combine` was non-const | made const
  LWG 436 | C++98 | it was unclear whether `Facet` can be cv-qualified | it can
      be const-qualified, but not volatile-qualified

### See also

- **(constructor)** — constructs a new locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/combine*
