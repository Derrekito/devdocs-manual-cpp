# std::num_get<CharT,InputIt>::~num_get

```cpp
protected: ~num_get();
```

Destructs a `std::num_get` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::num_get`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::num_get` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_num_get : public std::num_get<wchar_t>
{
    Destructible_num_get(std::size_t refs = 0) : num_get(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_num_get dc;
    // std::num_get<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/num_get/%7Enum_get*
