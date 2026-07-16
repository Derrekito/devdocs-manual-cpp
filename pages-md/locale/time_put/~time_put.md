# std::time_put<CharT,OutputIt>::~time_put

```cpp
protected: ~time_put();
```

Destructs a `std::time_put` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::time_put`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::time_put` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_time_put : public std::time_put<wchar_t>
{
    Destructible_time_put(std::size_t refs = 0) : time_put(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_time_put dc;
    // std::time_put<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/time_put/%7Etime_put*
