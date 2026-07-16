# std::basic_string<CharT,Traits,Allocator>::resize

Changes `size()` to `count`, actually mutating content: growing pads
the new tail with `CharT()` (`'\0'` for `char`) or with a given fill
character, and shrinking truncates, discarding the tail. This is
unlike `reserve`, which only grows capacity and never touches
`size()` or existing characters.

```cpp skip
s.resize(count);      // grow: pad with CharT(); shrink: truncate to count
s.resize(count, ch);  // grow: pad with ch; shrink: truncate to count
```

### What you provide

- **count** — the new size. If less than the current size, the string
  is truncated to its first `count` characters.
- **ch** — (second overload) the character used to fill newly added
  positions when growing. Omit it and grown positions are `CharT()`.

### Guarantees and costs

- Throws `std::length_error` if `count > max_size()`, plus whatever
  the allocator throws (typically `std::bad_alloc`).
- Strong exception safety: if it throws, the string is unchanged.
- `constexpr` since C++20.

### Gotchas

- `resize` mutates content (truncates or pads); `reserve` only affects
  capacity and leaves `size()` and every existing character alone —
  don't reach for the wrong one.
- Growing without a fill character inserts `'\0'` bytes for `char`,
  not spaces — printing the result as a C string stops at the first
  padded byte, so check `size()`/iterate instead of trusting `c_str()`.
- Shrinking loses data permanently; there's no "resize back" to
  recover truncated characters.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s = "Where is the end?";

    s.resize(8);
    std::cout << s << '\n';  // truncated to first 8 chars

    std::string t = "H";
    t.resize(8, 'a');
    std::cout << t << '\n';  // padded with 'a'

    t.resize(9);              // pad with CharT(), i.e. '\0'
    std::cout << t.size() << '\n';
    std::cout << (t.back() == char() ? "null pad" : "not null") << '\n';
}
```

```text
Where is
Haaaaaaa
9
null pad
```

### Reference

```cpp skip
void resize( size_type count );  // (until C++20)
constexpr void resize( size_type count );  // (since C++20)
void resize( size_type count, CharT ch );  // (until C++20)
constexpr void resize( size_type count, CharT ch );  // (since C++20)
```

If `count` is greater than the current size, characters are appended:
`CharT()` for the first overload, `ch` for the second. If `count` is
less than the current size, the string is reduced to its first
`count` elements. LWG 847 (applied to C++98) added the strong
exception safety guarantee retroactively.

### See also

- **size** / **length** — returns the number of characters
- **reserve** — grows capacity without changing size or content
- **shrink_to_fit** — reduces capacity to fit the current size

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/resize*
