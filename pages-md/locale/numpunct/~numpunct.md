# std::numpunct<CharT>::~numpunct

```cpp
protected: ~numpunct();
```

Destructs a `std::numpunct` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::numpunct`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::numpunct` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_numpunct : public std::numpunct<wchar_t>
{
    Destructible_numpunct(std::size_t refs = 0) : numpunct(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_numpunct dc;
    // std::numpunct<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/numpunct/%7Enumpunct*
