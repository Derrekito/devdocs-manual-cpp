# std::literals::string_view_literals::operator""sv

```cpp
constexpr std::string_view
    operator ""sv( const char* str, std::size_t len ) noexcept;  // (1) (since C++17)
constexpr std::u8string_view
    operator ""sv( const char8_t* str, std::size_t len ) noexcept;  // (2) (since C++20)
constexpr std::u16string_view
    operator ""sv( const char16_t* str, std::size_t len ) noexcept;  // (3) (since C++17)
constexpr std::u32string_view
    operator ""sv( const char32_t* str, std::size_t len ) noexcept;  // (4) (since C++17)
constexpr std::wstring_view
    operator ""sv( const wchar_t* str, std::size_t len ) noexcept;  // (5) (since C++17)
```

Forms a string view of a character literal.

1) Returns `std::string_view{str, len}`.

2) Returns `std::u8string_view{str, len}`.

3) Returns `std::u16string_view{str, len}`.

4) Returns `std::u32string_view{str, len}`.

5) Returns `std::wstring_view{str, len}`.

### Parameters

- **str** — pointer to the beginning of the raw character array literal
- **len** — length of the raw character array literal

### Return value

The `std::basic_string_view` literal.

### Notes

These operators are declared in the namespace
`std::literals::string_view_literals`, where both `literals` and
`string_view_literals` are inline namespaces. Access to these operators can be
gained with any of:

- `using namespace std::literals`,
- `using namespace std::string_view_literals`, or
- `using namespace std::literals::string_view_literals`.

### Example

```cpp
#include <iostream>
#include <string_view>
#include <typeinfo>

void print_each_character(const std::string_view sw)
{
    for (char c : sw)
        std::cout << (c == '\0' ? '@' : c);
    std::cout << '\n';
}

int main()
{
    using namespace std::literals;

    std::string_view s1 = "abc\0\0def";
    std::string_view s2 = "abc\0\0def"sv;

    std::cout << "s1.size(): " << s1.size() << "; s1: ";
    print_each_character(s1);
    std::cout << "s2.size(): " << s2.size() << "; s2: ";
    print_each_character(s2);

    std::cout << "substr(1, 4): " << "abcdef"sv.substr(1, 4) << '\n';

    auto value_type_info = []<typename T>(T)
    {
        using V = typename T::value_type;
        std::cout << "sizeof " << typeid(V).name() << ": " << sizeof(V) << '\n';
    };

    value_type_info("char A"sv);
    value_type_info(L"wchar_t ∀"sv);
    value_type_info(u8"char8_t ∆"sv);
    value_type_info(u"char16_t ∇"sv);
    value_type_info(U"char32_t ∃"sv);
    value_type_info(LR"(raw ⊞)"sv);
}
```

Possible output:

```text
s1.size(): 3; s1: abc
s2.size(): 8; s2: abc@@def
substr(1, 4): bcde
sizeof char: 1
sizeof wchar_t: 4
sizeof char8_t: 1
sizeof char16_t: 2
sizeof char32_t: 4
sizeof wchar_t: 4
```

### See also

- **(constructor)** — constructs a `basic_string_view` (public member function)
- **operator""s (C++14)** — converts a character array literal to `basic_string`
  (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/operator%22%22sv*
