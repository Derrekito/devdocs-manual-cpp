# std::moneypunct<CharT,International>::~moneypunct

```cpp
protected: ~moneypunct();
```

Destructs a `std::moneypunct` facet. This destructor is protected and virtual
(due to base class destructor being virtual). An object of type
`std::moneypunct`, like most facets, can only be destroyed when the last
`std::locale` object that implements this facet goes out of scope or if a
user-defined class is derived from `std::moneypunct` and implements a public
destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_moneypunct : public std::moneypunct<wchar_t>
{
    Destructible_moneypunct(std::size_t refs = 0) : moneypunct(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_moneypunct dc;
    // std::moneypunct<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/moneypunct/%7Emoneypunct*
