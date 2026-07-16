# std::basic_string<CharT,Traits,Allocator>::replace

Replaces a run of characters with new ones and returns `*this`. Two
independent choices combine into the overload set: how you name the
run to replace — `(pos, count)` or an iterator range
`(first, last)` — and what to replace it with — another string, a
slice of one, a C string, a repeated character, an iterator range, an
initializer list, or a `string_view`-like object.

```cpp skip
s.replace(pos, count, str);              // pos/count target, whole str
s.replace(first, last, str);             // iterator target, whole str
s.replace(pos, count, str, pos2, count2);// pos/count target, slice of str
s.replace(pos, count, cstr, count2);     // target replaced with count2 chars
s.replace(pos, count, cstr);             // target replaced with a C string
s.replace(pos, count, count2, ch);       // target replaced with count2 copies of ch
s.replace(first, last, first2, last2);   // both sides given as iterators
s.replace(first, last, {a, b, c});       // target replaced with an init list
s.replace(pos, count, string_view_like); // (since C++17)
```

### What you provide

- **pos, count** — the run to replace, as `[pos, pos + count)`
  (clamped to `size()`); `pos > size()` throws `std::out_of_range`.
- **first, last** — the run to replace, as a `const_iterator` range
  instead of `pos`/`count`. Must be a valid range into `*this`.
- **str, pos2, count2** — the replacement source, optionally sliced;
  an oversized `count2` clamps to `str.size()`.
- **cstr, count2 / cstr** — a raw buffer of `count2` characters (may
  hold embedded nulls), or a null-terminated C string.
- **count2, ch** — `count2` copies of `ch` as the replacement.
- **first2, last2 / ilist / t** — an iterator range, initializer
  list, or (since C++17) a `string_view`-convertible object as the
  replacement.

### Guarantees and costs

- Returns `*this`.
- Throws `std::out_of_range` for an out-of-bounds `pos` (or `pos2`),
  and `std::length_error` if the result would exceed `max_size()`.
  Strong exception safety: on throw, the string is unchanged.
- `[first, last)` (and `[begin(), first)`) must be a valid range —
  violating that is undefined behavior, not a thrown exception.
- All overloads `constexpr` since C++20.

### Gotchas

- Mixing the two target styles is a common slip: `replace(pos, count,
  ...)` takes indices, `replace(first, last, ...)` takes iterators —
  passing an iterator where an index is expected won't compile, but
  passing the wrong *kind* of range (e.g. iterators into a different
  string) is UB, not a diagnosed error.
- As with `append`, an oversized `count`/`count2` clamps silently;
  only an out-of-range `pos`/`pos2` throws.
- Replacing invalidates iterators/pointers/references into the string
  when the length changes or a reallocation occurs.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s = "The quick brown fox";

    s.replace(4, 5, "slow");                 // pos/count target
    std::cout << s << '\n';

    auto it = s.begin() + s.find("brown");
    s.replace(it, it + 5, "red");            // iterator target
    std::cout << s << '\n';

    s.replace(s.size() - 3, 3, 2, '!');      // replace "fox" with "!!"
    std::cout << s << '\n';
}
```

```text
The slow brown fox
The slow red fox
The slow red !!
```

### Reference

```cpp skip
basic_string& replace( size_type pos, size_type count,
                       const basic_string& str );
basic_string& replace( const_iterator first, const_iterator last,
                       const basic_string& str );
basic_string& replace( size_type pos, size_type count,
                       const basic_string& str,
                       size_type pos2, size_type count2 = npos );  // count2 default since C++14
basic_string& replace( size_type pos, size_type count,
                       const CharT* cstr, size_type count2 );
basic_string& replace( const_iterator first, const_iterator last,
                       const CharT* cstr, size_type count2 );
basic_string& replace( size_type pos, size_type count,
                       const CharT* cstr );
basic_string& replace( const_iterator first, const_iterator last,
                       const CharT* cstr );
basic_string& replace( size_type pos, size_type count,
                       size_type count2, CharT ch );
basic_string& replace( const_iterator first, const_iterator last,
                       size_type count2, CharT ch );
template< class InputIt >
basic_string& replace( const_iterator first, const_iterator last,
                       InputIt first2, InputIt last2 );
basic_string& replace( const_iterator first, const_iterator last,
                       std::initializer_list<CharT> ilist );  // (since C++11)
template< class StringViewLike >
basic_string& replace( size_type pos, size_type count,
                       const StringViewLike& t );  // (since C++17)
template< class StringViewLike >
basic_string& replace( const_iterator first, const_iterator last,
                       const StringViewLike& t );  // (since C++17)
template< class StringViewLike >
basic_string& replace( size_type pos, size_type count,
                       const StringViewLike& t,
                       size_type pos2, size_type count2 = npos );  // (since C++17)
```

All overloads `constexpr` since C++20. `InputIt` must meet
LegacyInputIterator. The `StringViewLike` overloads participate in
overload resolution only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`.

### See also

- **replace_with_range** — replaces a slice with a range (C++23)
- **regex_replace** — replaces regex matches with formatted text (C++11)
- **replace_if** — replaces values satisfying a predicate (algorithm)
- **substr** — returns a copy of a slice
- **append** — grows the string instead of replacing a slice

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/replace*
