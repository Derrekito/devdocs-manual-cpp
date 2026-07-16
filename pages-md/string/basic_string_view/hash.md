# std::hash<std::string_view>, std::hash<std::wstring_view>, std::hash<std::u8string_view>, std::hash<std::u16string_view>, std::hash<std::u32string_view>

```cpp
template<> struct hash<std::string_view>;  // (since C++17)
template<> struct hash<std::wstring_view>;  // (since C++17)
template<> struct hash<std::u8string_view>;  // (since C++20)
template<> struct hash<std::u16string_view>;  // (since C++17)
template<> struct hash<std::u32string_view>;  // (since C++17)
```

Template specializations of `std::hash` for the various view classes for hashing
views.

These hashes equal the hashes of corresponding `std::basic_string` classes: If S
is one of the standard basic_string types, SV is the corresponding string view
type, and s is an object of type S, then `std::hash<S>()(s) ==
std::hash<SV>()(SV(s))`.

### Example

```cpp
#include <iostream>
#include <string_view>
#include <unordered_set>
using namespace std::literals;

int main()
{
    std::cout << "\"A\"   #: " << std::hash<std::string_view>{}("A"sv) << '\n';
    std::cout << "L\"B\"  #: " << std::hash<std::wstring_view>{}(L"B"sv) << '\n';
    std::cout << "u8\"C\" #: " << std::hash<std::u8string_view>{}(u8"C"sv) << '\n';
    std::cout << "u\"D\"  #: " << std::hash<std::u16string_view>{}(u"D"sv) << '\n';
    std::cout << "U\"E\"  #: " << std::hash<std::u32string_view>{}(U"E"sv) << '\n';

    // std::hash for string_view family makes it possible to keep these view-types
    // in unordered_* associative containers, such as unordered_set. But ensure
    // the lifespan of referenced strings is no less than lifespan of the container,
    // i.e. no dangling references occurred.

    std::unordered_set stars{"Rigel"sv, "Capella"sv, "Vega"sv, "Arcturus"sv};

    for (std::string_view const& s : stars)
        std::cout << s << ' ';
    std::cout << '\n';
}
```

Possible output:

```text
"A"   #: 6919333181322027406
L"B"  #: 11959850520494268278
u8"C" #: 12432341034569643010
u"D"  #: 312659256970442235
U"E"  #: 18073225910249204957
Arcturus Vega Capella Rigel
```

### See also

- **hash (C++11)** — hash function object (class template)

- **std::hash<std::basic_string> (C++11)** — hash support for strings (class
  template specialization)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/hash*
