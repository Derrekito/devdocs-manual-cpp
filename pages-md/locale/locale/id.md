# std::locale::id

```cpp
class locale::id;
```

The class `std::locale::id` provides implementation-specific identification of a
locale facet. Each facet has a public static member named `id` of type
`std::locale::id` and each `std::locale` object maintains a list of facets it
implements, indexed by their `id`s.

Facets with the same `id` belong to the same facet category and replace each
other when added to a locale object.

### Member functions

- **(constructor)** — constructs a new id (public member function)
- **operator=** — copy assignment operator is deleted (public member function)

### Example

The following example shows how to construct a minimal custom facet.

```cpp
#include <iostream>
#include <locale>

struct myfacet : std::locale::facet
{
    myfacet(std::size_t refs = 0) : facet(refs) {}
    static ::std::locale::id id;
};

std::locale::id myfacet::id;

int main()
{
    std::locale myloc(std::locale(), new myfacet);
    std::cout << "has_facet<myfacet>(myloc) returns " << std::boolalpha
              << std::has_facet<myfacet>(myloc) << '\n';
}
```

Output:

```text
has_facet<myfacet>(myloc) returns true
```

### See also

- **facet** — the base class for all facet categories: each facet of any
  category is derived from this type (class)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/id*
