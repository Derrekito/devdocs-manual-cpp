# std::time_get<CharT,InputIt>::~time_get

```cpp
protected: ~time_get();
```

Destructs a `std::time_get` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::time_get`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::time_get` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_time_get : public std::time_get<wchar_t>
{
    Destructible_time_get(std::size_t refs = 0) : time_get(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_time_get dc;
    // std::time_get<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/time_get/%7Etime_get*
