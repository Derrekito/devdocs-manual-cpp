# std::basic_string

`std::string` is `std::basic_string<char>` — the alias almost everyone
reaches for in practice. The class template also backs `std::wstring`
(wide characters) and the Unicode-flavored `std::u8string` (C++20),
`std::u16string`, `std::u32string` (C++11), plus `std::pmr::` versions
of each (C++17) that use a polymorphic allocator. It's a dynamically
sized, growable sequence of characters stored contiguously (since
C++11), always followed by a trailing `CharT()` null terminator, so
`s.c_str()` can be handed to any C API expecting a `\0`-terminated
array.

```cpp skip
std::string s = "hello";              // construct from a literal
std::wstring w = L"hello";            // wide-character variant
s += " world";  s.push_back('!');     // grow at the end
s[i];  s.at(i);                       // unchecked / bounds-checked access
s.find("wor");  s.substr(pos, len);   // search / slice; npos on no match
s.c_str();  s.data();                 // null-terminated C string
s.starts_with(p);  s.ends_with(p);    // (since C++20)
s.contains(sub);                      // (since C++23)
```

### What you provide

- **CharT** — the character type: `char`, `wchar_t`, `char8_t`,
  `char16_t`, `char32_t`, or anything TrivialType and
  StandardLayoutType.
- **Traits** — the operations on that character type (comparison,
  length, ...); the default `std::char_traits<CharT>` is almost always
  right. `Traits::char_type` must name the same type as `CharT` or the
  program is ill-formed.
- **Allocator** — the memory allocator; the default `std::allocator<CharT>`
  is almost always right.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) / (destructor) | build / destroy the string |
| `operator=`, `assign` | replace the contents |
| `assign_range` (C++23) | assign from a range |
| `get_allocator` | the associated allocator |
| `at` | bounds-checked access, throws `std::out_of_range` |
| `operator[]` | unchecked access |
| `front`, `back` | first / last character |
| `data` | pointer to the characters (mutable since C++17) |
| `c_str` | null-terminated `const CharT*` |
| `operator basic_string_view` (C++17) | non-owning view of the whole string |
| `begin`/`cbegin`, `end`/`cend` | forward iterators |
| `rbegin`/`crbegin`, `rend`/`crend` | reverse iterators |
| `empty`, `size`/`length`, `max_size` | size queries |
| `reserve`, `capacity`, `shrink_to_fit` | manage allocated storage |
| `clear` | remove all characters |
| `insert`, `insert_range` (C++23) | insert characters |
| `erase` | remove characters |
| `push_back`, `pop_back` | add / remove the last character |
| `append`, `append_range` (C++23), `operator+=` | grow at the end |
| `replace`, `replace_with_range` (C++23) | overwrite a portion |
| `copy` | copy characters out to a raw buffer |
| `resize` | change the character count |
| `resize_and_overwrite` (C++23) | resize via callback, skipping value-init |
| `swap` | exchange contents with another string |
| `find`, `rfind` | locate a substring, forward / backward |
| `find_first_of`, `find_last_of` | locate any of a set of characters |
| `find_first_not_of`, `find_last_not_of` | first/last char *not* in a set |
| `compare` | three-way lexicographic comparison |
| `starts_with`, `ends_with` (C++20) | prefix / suffix test |
| `contains` (C++23) | substring test |
| `substr` | copy of a slice |
| `npos` [static] | "not found" / "to the end" sentinel value |

Non-members: `operator+` (concatenation); the comparison operators
(`<=>` since C++20 replaces the individual `==`/`!=`/`<`/... overloads);
`std::swap`; `std::erase`/`std::erase_if` (C++20); `operator<<` /
`operator>>` and `getline` for stream I/O (see the `getline` page);
`std::stoi`/`stol`/`stoll`/`stoul`/`stoull`/`stof`/`stod`/`stold`
(C++11, string to number — see the `stol` page); `to_string` /
`to_wstring` (C++11, number to string); the `operator""s` literal
(C++14); `std::hash<basic_string>` (C++11).

### Guarantees and costs

- Storage is contiguous since C++11 — `&s[0]` can be passed anywhere a
  pointer to the first element of a null-terminated `CharT` array is
  expected. Before C++11 this wasn't required (a defect report, LWG
  530, applied it retroactively to C++98).
- A trailing `CharT()` always follows the last character (since C++11).
- Satisfies AllocatorAwareContainer, SequenceContainer, and (since
  C++17) ContiguousContainer — though a customized allocator's
  `construct`/`destroy` are never actually invoked for element
  construction/destruction, regardless of what the standard technically
  required before C++23.
- If `Traits::char_type` or `Allocator::char_type` differs from
  `CharT`, the program is ill-formed — a compile error, not undefined
  behavior (corrected by a defect report, LWG 2994/P1148R0; earlier
  wording made it UB).
- Since C++20, member functions are `constexpr`, so `std::string` can
  be used inside constant expressions — but a `std::string` object
  generally can't itself be a `constexpr` value, because its
  dynamically allocated storage must be released within the same
  constant evaluation that created it.

### Gotchas

- A `CharT`/`Traits`/`Allocator` combination with mismatched
  `char_type` fails to *compile* — that's a template-instantiation
  error, not a runtime bug to chase down.
- `operator basic_string_view` (C++17) lets a `std::string` convert
  implicitly to `std::string_view` in many contexts (overload
  resolution, `auto` deduction through a function taking
  `string_view`); the conversion itself is free, but any `string_view`
  you keep around from it is only valid as long as the original string
  is.
- A custom `Traits` type changes what counts as "equal" or "less than"
  for `find`, `compare`, and `==` — even though construction and most
  other members look and behave identically to the default
  `std::char_traits`.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    using namespace std::literals;

    std::string str1 = "hello";
    auto str2 = "world"s;
    std::string str3 = str1 + " " + str2;
    std::cout << str3 << '\n';

    auto pos = str3.find(' ');
    str1 = str3.substr(pos + 1);   // the part after the space
    str2 = str3.substr(0, pos);    // the part up to the space
    std::cout << str1 << ' ' << str2 << '\n';

    str1[0] = 'W';
    std::cout << str1 << '\n';
}
```

```text
hello world
world hello
World
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>,
    class Allocator = std::allocator<CharT>
> class basic_string;  // (1)
namespace pmr {
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> using basic_string =
    std::basic_string<CharT, Traits, std::pmr::polymorphic_allocator<CharT>>;
}  // (2) (since C++17)
```

Formally: elements are non-array objects of TrivialType and
StandardLayoutType; for any `basic_string` `s` and `n` in
`[0, s.size())`, `&*(s.begin() + n) == &*s.begin() + n` (contiguity),
and `*(s.begin() + s.size())` has value `CharT()`. Feature-test macros
(values as of their introducing standard): `__cpp_lib_string_udls`
(`201304L`, C++14, `operator""s`); `__cpp_lib_starts_ends_with`
(`201711L`, C++20); `__cpp_lib_constexpr_string` (`201907L`, C++20);
`__cpp_lib_char8_t` (`201907L`, C++20, `std::u8string`);
`__cpp_lib_erase_if` (`202002L`, C++20); `__cpp_lib_string_contains`
(`202011L`, C++23); `__cpp_lib_string_resize_and_overwrite`
(`202110L`, C++23); `__cpp_lib_containers_ranges` (`202202L`, C++23,
range-accepting constructors and modifiers).

### See also

- **basic_string_view** (C++17) — read-only, non-owning view over the
  same kind of character sequence

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string*
