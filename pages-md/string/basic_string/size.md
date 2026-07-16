# std::basic_string<CharT,Traits,Allocator>::size, length

Returns the number of `CharT` elements in the string — exactly
`std::distance(begin(), end())`. `size()` and `length()` are the same
function; `size()` matches the rest of the container interface,
`length()` is the older string-specific name. Pick whichever reads
better and never worry about a difference, because there isn't one.

```cpp skip
s.size();    // number of CharT elements
s.length();  // identical to size()
```

### What you provide

Nothing — takes no arguments.

### Guarantees and costs

- Constant complexity since C++11 (unspecified before that, though
  every real implementation was already O(1)).
- `noexcept`; `constexpr` since C++20.
- Counts `CharT` storage units, not "characters" in the human sense:
  for `std::string` those units are bytes, which is not the same as
  Unicode code points under a multibyte encoding like UTF-8 — one
  code point can span several bytes.

### Gotchas

- On a UTF-8 `std::string`, `size()` counts encoded bytes: a string of
  8 Japanese code points can report `size() == 24`. Use
  `size() == 8` only when you built it from a fixed-width encoding
  (`std::u32string`, or `std::u16string` for BMP-only text).
- `size()` and `length()` are 100% interchangeable — reaching for one
  over the other is a style choice, not a correctness one.

### Example

```cpp c++20
#include <iostream>
#include <string>

int main()
{
    std::string s("Exemplar");
    std::cout << s.size() << ' ' << s.length() << '\n';

    std::u32string a(U"ハロー・ワールド");  // 8 code points
    std::cout << a.size() << '\n';          // 8 code units in UTF-32

    std::string c("ハロー・ワールド");  // 8 code points
    std::cout << c.size() << '\n';       // 24 code units in UTF-8
}
```

```text
8 8
8
24
```

### Reference

```cpp skip
size_type size() const;  // (until C++11)
size_type size() const noexcept;  // (since C++11) (until C++20)
constexpr size_type size() const noexcept;  // (since C++20)
size_type length() const;  // (until C++11)
size_type length() const noexcept;  // (since C++11) (until C++20)
constexpr size_type length() const noexcept;  // (since C++20)
```

Both return `std::distance(begin(), end())`; the standard defines
`length()` as a synonym of `size()`, not vice versa, but the two are
symmetric in practice.

### See also

- **empty** — checks whether the string has zero characters
- **max_size** — returns the largest size the string could reach
- **capacity** — returns the current storage capacity
- **resize** — changes the number of characters

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/size*
