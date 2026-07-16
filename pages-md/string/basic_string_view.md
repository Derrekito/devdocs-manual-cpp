# std::basic_string_view

`std::string_view` (C++17) is `std::basic_string_view<char>` — a
non-owning, read-only view over a contiguous run of characters:
essentially a `{pointer, length}` pair, with no allocation and no
ownership. That makes one rule dominate everything else about this
type: **a `string_view` must never outlive the buffer it points into**
(a `std::string`, a string literal, a `char` array). Nothing stops you
from creating a dangling one, and reading through it afterward is
undefined behavior. The family also includes `std::wstring_view`,
`std::u8string_view` (C++20), `std::u16string_view`, and
`std::u32string_view` (all C++17 except `u8string_view`).

```cpp skip
std::string_view sv = "hello";        // from a literal, no copy
std::string_view sv = some_string;    // from a std::string, no copy
sv.substr(pos, len);                  // another view, still no copy
sv.find("lo");                        // returns npos on no match
sv.starts_with(p);  sv.ends_with(p);  // (since C++20)
sv.remove_prefix(n);  sv.remove_suffix(n);   // shrink the view in place
```

### What you provide

- **CharT** — the character type.
- **Traits** — the CharTraits class for that character type; as with
  `basic_string`, `Traits::char_type` must name the same type as
  `CharT` or the program is ill-formed.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor), `operator=` | build / reassign a view |
| `begin`/`cbegin`, `end`/`cend` | forward iterators |
| `rbegin`/`crbegin`, `rend`/`crend` | reverse iterators |
| `operator[]`, `at` | unchecked / bounds-checked access |
| `front`, `back` | first / last character |
| `data` | pointer to the viewed characters |
| `size`/`length`, `max_size`, `empty` | size queries |
| `remove_prefix(n)`, `remove_suffix(n)` | shrink from either end, in place |
| `swap` | exchange what two views point at |
| `copy` | copy characters out to a raw buffer |
| `substr` | another view over a slice |
| `compare` | three-way lexicographic comparison |
| `starts_with`, `ends_with` (C++20) | prefix / suffix test |
| `contains` (C++23) | substring test |
| `find`, `rfind` | locate a substring, forward / backward |
| `find_first_of`, `find_last_of` | locate any of a set of characters |
| `find_first_not_of`, `find_last_not_of` | first/last char *not* in a set |
| `npos` [static] | "not found" / "to the end" sentinel value |

Non-members: the comparison operators (C++17; `<=>` since C++20
replaces the individual overloads); `operator<<` (C++17, stream
output); the `operator""sv` literal (C++17);
`std::hash<basic_string_view>` and its typedef specializations
(C++17, `u8string_view` in C++20).

### Guarantees and costs

- Every operation is O(1) or O(view size) and never touches the
  underlying buffer: `substr`, `remove_prefix`, `remove_suffix` just
  adjust the pointer/length, they don't copy characters.
- `iterator` and `const_iterator` are the same type — a view only ever
  looks at `const` data, so there's no mutable-iterator form.
- TriviallyCopyable is a formal requirement since C++23, but every
  existing implementation already satisfied it — passing or storing a
  `string_view` by value is always cheap.

### Gotchas

- The classic dangling trap: binding a view to a temporary
  `std::string` (an `operator""s` result, or a function returning
  `std::string` by value) compiles cleanly and dangles the moment that
  temporary is destroyed at the end of the full expression.
- `Traits::char_type` must match `CharT` exactly, same constraint (and
  same ill-formed-not-UB history) as `basic_string`.
- A copy of a `string_view` is shallow: it duplicates the
  pointer/length, not the characters. Mutating the underlying buffer
  through any other path changes what every copy of the view sees.

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view unicode[] { "▀▄─", "▄▀─", "▀─▄", "▄─▀" };

    for (int y{}, p{}; y != 4; ++y, p = ((p + 1) % 4))
    {
        for (int x{}; x != 8; ++x)
            std::cout << unicode[p];
        std::cout << '\n';
    }
}
```

```text
▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─
▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─
▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄
▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀
```

### Reference

```cpp skip
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_string_view;  // (since C++17)

template< class CharT, class Traits >
inline constexpr bool
    ranges::enable_borrowed_range<std::basic_string_view<CharT, Traits>> = true;  // (since C++20)
template< class CharT, class Traits >
inline constexpr bool
    ranges::enable_view<std::basic_string_view<CharT, Traits>> = true;  // (since C++20)
```

Formally: describes an object referring to a constant contiguous
sequence of `CharT`, with the first element at position zero. A
typical implementation holds only a pointer to constant `CharT` and a
size. The `ranges::enable_borrowed_range` specialization makes
`basic_string_view` satisfy `borrowed_range`; `ranges::enable_view`
makes it satisfy `view` (both C++20) — together these let a
`string_view` be used directly in range-based algorithm pipelines
without the pipeline treating it as something that could dangle the
way a temporary container would. Feature-test macro:
`__cpp_lib_string_view` — `201606L` (C++17, the type itself),
`201803L` (C++20, constexpr iterator support);
`__cpp_lib_string_contains` (`202011L`, C++23, `contains`).

### See also

- **basic_string** — the owning counterpart; stores and manipulates
  its own character sequence
- **span** (C++20) — the equivalent non-owning view for any contiguous
  sequence of objects, not just characters
- **initializer_list** (C++11) — another lightweight, non-owning view
  type, over a temporary array from list-initialization

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view*
