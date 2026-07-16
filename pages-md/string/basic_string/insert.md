# std::basic_string<CharT,Traits,Allocator>::insert

Inserts characters into the string. The overload set is large, but it
splits cleanly along two axes: **where** (an `index` counted from the
start, or an iterator `pos`) and **what** (a repeated char, a C
string, a `basic_string` or substring of one, a range, an
initializer list, or — since C++17 — anything convertible to
`string_view`).

```cpp skip
s.insert(index, count, ch);       // count copies of ch, at index
s.insert(index, cstr);            // null-terminated C string, at index
s.insert(index, cstr, count);     // first count chars of cstr, at index
s.insert(index, str);             // another basic_string, at index
s.insert(index, str, s_index, count);  // str.substr(s_index, count), at index
s.insert(index, string_view_like);     // (since C++17) anything convertible
s.insert(pos, ch);                // one char, before iterator pos
s.insert(pos, count, ch);         // count copies of ch, before pos
s.insert(pos, first, last);       // range [first, last), before pos
s.insert(pos, ilist);             // initializer_list, before pos
```

### What you provide

- **index** — position to insert at, counted from `0`; throws if it
  exceeds `size()`.
- **pos** — iterator to insert before; must be valid on `*this`, or
  behavior is undefined.
- **ch / count** — a character and how many copies of it to insert.
- **s / cstr** — a null-terminated `const CharT*`, or (with an
  explicit `count`) a pointer to `count` characters that may contain
  embedded nulls.
- **str / s_index** — a `basic_string` to insert whole, or (with
  `s_index`, `count`) the substring `str.substr(s_index, count)`.
- **first, last** — an iterator range whose value type meets
  LegacyInputIterator.
- **ilist** — a `std::initializer_list<CharT>`.
- **t / t_index** — (since C++17) anything implicitly convertible to
  `std::basic_string_view<CharT, Traits>` but not to `const CharT*`;
  optionally a subview `[t_index, t_index + count)` of it.

### Guarantees and costs

- Index-based overloads (1-5, 10, 11) return `*this`.
- Iterator-based overloads (6-9) return an iterator to the first
  inserted character, or `pos` unchanged if nothing was inserted
  (`count == 0`, `first == last`, or an empty `ilist`).
- Throws `std::out_of_range` if `index > size()` (and, for the
  substring/subview forms, if `s_index`/`t_index` exceeds the source's
  size). Throws `std::length_error` if the result would exceed
  `max_size()`.
- Strong exception safety: if any overload throws, `*this` is
  unchanged.
- `constexpr` since C++20.

### Gotchas

- The `count`-qualified C-string overload copies exactly `count`
  characters, embedded nulls and all; passing a `count` that reads
  past the buffer is undefined behavior, just like any bad-range bug.
- Any insertion can reallocate, invalidating every iterator, pointer,
  and reference into the string — don't hold onto old ones across a
  call.
- The `string_view`-like overload only participates in overload
  resolution when `t` converts to `string_view` but *not* to
  `const CharT*`; this is why passing a `const char*` still resolves
  to the C-string overload instead.

### Example

```cpp c++17
#include <iostream>
#include <string>

using namespace std::string_literals;

int main()
{
    std::string s = "xmplr";

    s.insert(0, 1, 'E');            // index, count, ch
    std::cout << s << '\n';

    s.insert(2, "e");               // index, C string
    std::cout << s << '\n';

    s.insert(6, "a"s);              // index, basic_string
    std::cout << s << '\n';

    s.insert(s.cbegin() + 5, ' ');  // pos, ch
    std::cout << s << '\n';
}
```

```text
Exmplr
Exemplr
Exemplar
Exemp lar
```

### Reference

```cpp skip
basic_string& insert( size_type index, size_type count, CharT ch );  // (until C++20)
constexpr basic_string&
    insert( size_type index, size_type count, CharT ch );  // (since C++20)
basic_string& insert( size_type index, const CharT* s );  // (until C++20)
constexpr basic_string& insert( size_type index, const CharT* s );  // (since C++20)
basic_string&
    insert( size_type index, const CharT* s, size_type count );  // (until C++20)
constexpr basic_string& insert( size_type index,
                                const CharT* s, size_type count );  // (since C++20)
basic_string& insert( size_type index, const basic_string& str );  // (until C++20)
constexpr basic_string&
    insert( size_type index, const basic_string& str );  // (since C++20)
basic_string& insert( size_type index, const basic_string& str,
                      size_type s_index, size_type count );  // (until C++14)
basic_string& insert( size_type index, const basic_string& str,
                      size_type s_index,
                      size_type count = npos );  // (since C++14) (until C++20)
constexpr basic_string& insert( size_type index, const basic_string& str,
                                size_type s_index,
                                size_type count = npos );  // (since C++20)
iterator insert( iterator pos, CharT ch );  // (until C++11)
iterator insert( const_iterator pos, CharT ch );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos, CharT ch );  // (since C++20)
void insert( iterator pos, size_type count, CharT ch );  // (until C++11)
iterator insert( const_iterator pos, size_type count,
                 CharT ch );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos, size_type count,
                           CharT ch );  // (since C++20)
template< class InputIt >
void insert( iterator pos, InputIt first, InputIt last );  // (until C++11)
template< class InputIt >
iterator insert( const_iterator pos, InputIt first,
                 InputIt last );  // (since C++11) (until C++20)
template< class InputIt >
constexpr iterator insert( const_iterator pos, InputIt first,
                           InputIt last );  // (since C++20)
iterator insert( const_iterator pos,
                 std::initializer_list<CharT> ilist );  // (since C++11) (until C++20)
constexpr iterator insert( const_iterator pos,
                           std::initializer_list<CharT> ilist );  // (since C++20)
template< class StringViewLike >
basic_string& insert( size_type index,
                      const StringViewLike& t );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& insert( size_type index,
                                const StringViewLike& t );  // (since C++20)
template< class StringViewLike >
basic_string& insert( size_type index, const StringViewLike& t,
                      size_type t_index,
                      size_type count = npos );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& insert( size_type index, const StringViewLike& t,
                                size_type t_index,
                                size_type count = npos );  // (since C++20)
```

`InputIt` must meet LegacyInputIterator; the range overload (8) does
not participate in overload resolution otherwise. The `StringViewLike`
overloads (10, 11) participate only if `t` converts to
`std::basic_string_view<CharT, Traits>` but not to `const CharT*`, and
insert as if via `insert(index, sv.data(), sv.size())` on the
(sub)view. (LWG 7) overload (8) originally cited the wrong overload
in its as-if description; corrected to overload (4). (LWG 2946)
overload (10) was made a template to avoid ambiguity.

### See also

- **erase** — removes characters from the string
- **append** — appends characters to the end
- **push_back** — appends a single character to the end
- **insert_range** (C++23) — inserts a range of characters
- **replace** — replaces a range of characters with new content

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/insert*
