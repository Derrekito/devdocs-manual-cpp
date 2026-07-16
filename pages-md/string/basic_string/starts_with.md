# std::basic_string<CharT,Traits,Allocator>::starts_with

Checks whether the string begins with a given prefix — a
`string_view`-convertible object, a single character, or a
null-terminated C string. Available since **C++20**; this page covers
`starts_with`, its mirror `ends_with` checks the tail instead, and
C++23 adds `contains` for "anywhere in the string."

```cpp skip
s.starts_with(sv);    // prefix is a string_view (or convertible to one)
s.starts_with(ch);    // prefix is a single character
s.starts_with(cstr);  // prefix is a null-terminated C string
```

### What you provide

- **sv** — a `std::basic_string_view<CharT, Traits>`, including
  another `basic_string` (implicitly convertible).
- **ch** — a single character.
- **s** — a null-terminated `const CharT*`.

### Guarantees and costs

- Returns `true` if the string begins with the prefix, `false`
  otherwise; an empty prefix always matches.
- All three overloads are equivalent to calling `starts_with` on a
  `string_view` over the same data — no allocation.
- The `sv` and `ch` overloads are `noexcept`; the C-string overload
  can throw only from computing its length. `constexpr` throughout.

### Gotchas

- C++20 only — on an older standard, use
  `s.compare(0, prefix.size(), prefix) == 0` or
  `s.rfind(prefix, 0) == 0` instead.
- Case-sensitive: `"Hello".starts_with("hello")` is `false`. There's
  no case-insensitive overload.
- Don't confuse with `contains` (C++23), which matches the prefix
  *anywhere*, not just at the start.

### Example

```cpp c++20
#include <cassert>
#include <string>
#include <string_view>

int main()
{
    using namespace std::literals;

    const auto str = "Hello, C++20!"s;

    assert(str.starts_with("He"sv));    // string_view prefix
    assert(!str.starts_with("he"sv));   // case-sensitive
    assert(str.starts_with('H'));       // single character
    assert(str.starts_with("He"));      // C string
}
```

```text

```

### Reference

```cpp skip
constexpr bool
    starts_with( std::basic_string_view<CharT,Traits> sv ) const noexcept;  // (1) (since C++20)
constexpr bool
    starts_with( CharT ch ) const noexcept;  // (2) (since C++20)
constexpr bool
    starts_with( const CharT* s ) const;  // (3) (since C++20)
```

All three overloads effectively return
`std::basic_string_view<CharT, Traits>(data(), size()).starts_with(x)`.
Feature-test macro `__cpp_lib_starts_ends_with` (value `201711L`)
signals availability.

### See also

- **ends_with** — checks if the string ends with a suffix (C++20)
- **contains** — checks if the string contains a substring anywhere (C++23)
- **compare** — compares two strings
- **substr** — returns a substring

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/starts_with*
