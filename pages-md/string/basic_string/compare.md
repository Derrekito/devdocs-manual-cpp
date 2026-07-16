# std::basic_string<CharT,Traits,Allocator>::compare

Lexicographically compares this string (or a slice of it) against
another string, C string, or `string_view`-like object. Returns
`< 0` if `*this` sorts before the argument, `0` if they're equal, and
`> 0` if it sorts after — three-way, like `strcmp`. If you only need
"are these equal?" or "is A less than B?", use `==` or `<` instead;
they're simpler to read and don't require reasoning about sign.

```cpp skip
s.compare(other);                      // whole string vs whole string
s.compare(pos1, count1, other);        // a slice of *this vs whole other
s.compare(pos1, count1, other, pos2, count2);  // slice vs slice
s.compare(cstr);                       // vs a C string
s.compare(pos1, count1, cstr);         // slice vs a C string
s.compare(pos1, count1, cstr, count2); // slice vs count2 chars of cstr
s.compare(string_view_like);           // (since C++17)
```

### What you provide

- **pos1, count1** — optional slice `[pos1, pos1 + count1)` of
  `*this`; an oversized `count1` clamps to `size()`.
- **str / cstr / t** — what to compare against: another
  `basic_string`, a null-terminated `const CharT*`, or (since C++17)
  anything convertible to `std::basic_string_view`.
- **pos2, count2** — optional slice of the argument, same clamping
  rule.

### Guarantees and costs

- Returns negative/zero/positive exactly like `strcmp`: compares
  `min(count1, count2)` characters first; if that's a tie, the
  shorter sequence sorts first, and equal-length equal-content
  sequences compare `0`.
- Overloads taking a `pos1`/`pos2` throw `std::out_of_range` if that
  position exceeds the corresponding string's size. Strong exception
  safety throughout.
- Not locale-sensitive with the default `char_traits`; for
  locale-aware comparison use `std::collate::compare`.
- `constexpr` since C++20; the whole-string overload is `noexcept`
  since C++11.

### Gotchas

- The return value is a *sign*, not a distance — never treat it as a
  character offset or a count of differing characters.
- `compare` is the wrong tool for a simple equality check: `a == b`
  is clearer and just as fast. Reach for `compare` when you need
  ordering, a slice-vs-slice comparison, or a three-way result to
  store.
- With slices, an oversized `count1`/`count2` silently clamps to the
  end of the string rather than erroring; only an out-of-range `pos`
  throws.

### Example

```cpp
#include <iostream>
#include <string>

void show(int id, int r)
{
    std::cout << id << ") " << (r < 0 ? "less" : r > 0 ? "greater" : "equal")
              << '\n';
}

int main()
{
    std::string batman{"Batman"};
    std::string superman{"Superman"};

    show(1, batman.compare(superman));             // whole strings
    show(2, batman.compare(3, 3, superman));        // "man" vs "Superman"
    show(3, batman.compare(3, 3, superman, 5, 3));  // "man" vs "man"
    show(4, batman.compare("Superman"));            // vs a C string
}
```

```text
1) less
2) greater
3) equal
4) less
```

### Reference

```cpp skip
int compare( const basic_string& str ) const;
int compare( size_type pos1, size_type count1,
             const basic_string& str ) const;
int compare( size_type pos1, size_type count1,
             const basic_string& str,
             size_type pos2, size_type count2 = npos ) const;  // count2 default since C++14
int compare( const CharT* s ) const;
int compare( size_type pos1, size_type count1,
             const CharT* s ) const;
int compare( size_type pos1, size_type count1,
             const CharT* s, size_type count2 ) const;
template< class StringViewLike >
int compare( const StringViewLike& t ) const noexcept(/* see below */);  // (since C++17)
template< class StringViewLike >
int compare( size_type pos1, size_type count1,
             const StringViewLike& t ) const;  // (since C++17)
template< class StringViewLike >
int compare( size_type pos1, size_type count1,
             const StringViewLike& t,
             size_type pos2, size_type count2 = npos ) const;  // (since C++17)
```

All overloads `constexpr` since C++20. The `StringViewLike` overloads
participate in overload resolution only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`.
Formally: compute `rlen = min(count1, count2)`, compare the first
`rlen` characters via `Traits::compare`; if that's `0`, the shorter
operand (by total size) is *less*, and equal sizes give `0`.

### Notes

`std::basic_string` provides `<`, `<=`, `==`, `>`, `>=` for the common
case where you don't need a three-way result.

### See also

- **operator==**, **operator<**, **operator<=>** — simpler binary
  comparisons (`<=>` gives ordering since C++20)
- **substr** — returns a copy of a slice
- **lexicographical_compare** — three-way compare over arbitrary ranges
- **compare** — the non-owning `std::basic_string_view` equivalent

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/compare*
