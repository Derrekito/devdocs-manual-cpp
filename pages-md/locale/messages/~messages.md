# std::messages<CharT>::~messages

```cpp
protected: ~messages();
```

Destructs a `std::messages` facet. This destructor is protected and virtual (due
to base class destructor being virtual). An object of type `std::messages`, like
most facets, can only be destroyed when the last `std::locale` object that
implements this facet goes out of scope or if a user-defined class is derived
from `std::messages` and implements a public destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_messages : public std::messages<wchar_t>
{
    Destructible_messages(std::size_t refs = 0) : messages(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_messages dc;
    // std::messages<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/messages/%7Emessages*
