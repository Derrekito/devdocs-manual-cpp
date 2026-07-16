# std::has_facet

```cpp
template< class Facet >
bool has_facet( const locale& loc ) throw();  // (until C++11)
template< class Facet >
bool has_facet( const locale& loc ) noexcept;  // (since C++11)
```

Checks if the locale `loc` implements the facet `Facet`.

The program is ill-formed if Facet is not a facet or it is a volatile-qualified
facet.

### Parameters

- **loc** — the locale object to query

### Return value

Returns `true` if the facet `Facet` was installed in the locale `loc`, `false`
otherwise.

### Notes

`std::has_facet` must return `true` for all locales `loc` if `Facet` is one of
the standard facets given here.

### Example

```cpp
#include <iostream>
#include <locale>

// minimal custom facet
struct myfacet : public std::locale::facet
{
    static std::locale::id id;
};

std::locale::id myfacet::id;

int main()
{
    // loc is a "C" locale with myfacet added
    std::locale loc(std::locale::classic(), new myfacet);
    std::cout << std::boolalpha
              << "Can loc classify chars? "
              << std::has_facet<std::ctype<char>>(loc) << '\n'
              << "Can loc classify char32_t? "
              << std::has_facet<std::ctype<char32_t>>(loc) << '\n'
              << "Does loc implement myfacet? "
              << std::has_facet<myfacet>(loc) << '\n';
}
```

Output:

```text
Can loc classify chars? true
Can loc classify char32_t? false
Does loc implement myfacet? true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 436 | C++98 | it was unclear whether `Facet` can be cv-qualified | it can
      be const-qualified, but not volatile-qualified

### See also

- **locale** — set of polymorphic facets that encapsulate cultural differences
  (class)
- **use_facet** — obtains a facet from a locale (function template)

---
*Source: https://en.cppreference.com/w/cpp/locale/has_facet*
