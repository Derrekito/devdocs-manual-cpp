# std::hash<std::basic_string>

```cpp
template< class A >
struct hash<std::basic_string<char, std::char_traits<char>, A>>;  // (1) (since C++11)
template< class A >
struct hash<std::basic_string<char16_t, std::char_traits<char16_t>, A>>;  // (2) (since C++11)
template< class A >
struct hash<std::basic_string<char32_t, std::char_traits<char32_t>, A>>;  // (3) (since C++11)
template< class A >
struct hash<std::basic_string<wchar_t, std::char_traits<wchar_t>, A>>;  // (4) (since C++11)
template< class A >
struct hash<std::basic_string<char8_t, std::char_traits<char8_t>, A>>;  // (5) (since C++20)
```

The template specializations of `std::hash` for the various string classes allow
users to obtain hashes of strings.

These hashes equal the hashes of corresponding `std::basic_string_view` classes:
If `S` is one of these string types, `SV` is the corresponding string view type,
and `s` is an object of type `S`, then `std::hash<S>()(s) ==
std::hash<SV>()(SV(s))`.
*(since C++17)*

### Example

The following code shows one possible output of a hash function used on a
string:

```cpp
#include <functional>
#include <iostream>
#include <memory_resource>
#include <string>
#include <string_view>
using namespace std::literals;

int main()
{
    auto sv = "Stand back! I've got jimmies!"sv;
    std::string s(sv);
    std::pmr::string pmrs(sv); // use default allocator

    std::cout << std::hash<std::string_view>{}(sv) << '\n';
    std::cout << std::hash<std::string>{}(s) << '\n';
    std::cout << std::hash<std::pmr::string>{}(pmrs) << '\n';
}
```

Possible output:

```text
3544599705012401047
3544599705012401047
3544599705012401047
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3705 | C++11 | hash support for `std::basic_string` with customized
      allocators was not enabled | enabled

### See also

- **hash (C++11)** — hash function object (class template)
-
  **std::hash<std::string_view>std::hash<std::wstring_view>std::hash<std::u8string_view>std::hash<std::u16string_view>std::hash<std::u32string_view>
  (C++17)(C++17)(C++20)(C++17)(C++17)** — hash support for string views (class
  template specialization)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/hash*
