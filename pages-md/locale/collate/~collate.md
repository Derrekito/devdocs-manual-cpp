# std::collate<CharT>::~collate

```cpp
protected: ~collate();
```

Destructs a `std::collate` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::collate`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::collate` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_collate : public std::collate<wchar_t>
{
    Destructible_collate(std::size_t refs = 0) : collate(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_collate dc;
    // std::collate<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/collate/%7Ecollate*
