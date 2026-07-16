# std::money_put<CharT,OutputIt>::~money_put

```cpp
protected: ~money_put();
```

Destructs a `std::money_put` facet. This destructor is protected and virtual
(due to base class destructor being virtual). An object of type
`std::money_put`, like most facets, can only be destroyed when the last
`std::locale` object that implements this facet goes out of scope or if a
user-defined class is derived from `std::money_put` and implements a public
destructor.

### Example

```cpp
#include <iostream>
#include <locale>

struct Destructible_money_put : public std::money_put<wchar_t>
{
    Destructible_money_put(std::size_t refs = 0) : money_put(refs) {}
    // note: the implicit destructor is public
};

int main()
{
    Destructible_money_put dc;
    // std::money_put<wchar_t> c; // compile error: protected destructor
}
```

---
*Source: https://en.cppreference.com/w/cpp/locale/money_put/%7Emoney_put*
