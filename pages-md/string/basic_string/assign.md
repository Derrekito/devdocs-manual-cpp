# std::basic_string<CharT,Traits,Allocator>::assign

```cpp
basic_string& assign( size_type count, CharT ch );  // (until C++20)
constexpr basic_string& assign( size_type count, CharT ch );  // (since C++20)
basic_string& assign( const basic_string& str );  // (until C++20)
constexpr basic_string& assign( const basic_string& str );  // (since C++20)
basic_string& assign( const basic_string& str,
                      size_type pos, size_type count );  // (until C++14)
basic_string& assign( const basic_string& str,
                      size_type pos, size_type count = npos);  // (since C++14) (until C++20)
constexpr basic_string& assign( const basic_string& str,
                                size_type pos, size_type count = npos);  // (since C++20)
basic_string& assign( basic_string&& str ) noexcept(/* see below */);  // (since C++11) (until C++20)
constexpr basic_string& assign( basic_string&& str )
    noexcept(/* see below */);  // (since C++20)
basic_string& assign( const CharT* s, size_type count );  // (until C++20)
constexpr basic_string& assign( const CharT* s, size_type count );  // (since C++20)
basic_string& assign( const CharT* s );  // (until C++20)
constexpr basic_string& assign( const CharT* s );  // (since C++20)
template< class InputIt >
basic_string& assign( InputIt first, InputIt last );  // (until C++20)
template< class InputIt >
constexpr basic_string& assign( InputIt first, InputIt last );  // (since C++20)
basic_string& assign( std::initializer_list<CharT> ilist );  // (since C++11) (until C++20)
constexpr basic_string& assign( std::initializer_list<CharT> ilist );  // (since C++20)
template< class StringViewLike >
basic_string& assign( const StringViewLike& t );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& assign( const StringViewLike& t );  // (since C++20)
template< class StringViewLike >
basic_string& assign( const StringViewLike& t,
                      size_type pos, size_type count = npos);  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string& assign( const StringViewLike& t,
                                size_type pos, size_type count = npos);  // (since C++20)
```

Replaces the contents of the string.

1) Replaces the contents with `count` copies of character `ch`.

2) Replaces the contents with a copy of `str`. Equivalent to `*this = str;`. In
   particular, allocator propagation may take place.(since C++11)

3) Replaces the contents with a substring `[``pos``,``pos + count``)` of `str`.
   If the requested substring lasts past the end of the string, or if `count ==
   npos`, the resulting substring is `[``pos``,``str.size()``)`. If `pos >
   str.size()`, `std::out_of_range` is thrown.

4) Replaces the contents with those of `str` using move semantics. Equivalent to
   `*this = std::move(str)`. In particular, allocator propagation may take
   place.

5) Replaces the contents with copies of the characters in the range `[``s``,``s
   + count``)`. This range can contain null characters.

6) Replaces the contents with those of null-terminated character string pointed
   to by `s`. The length of the string is determined by the first null character
   using `Traits::length(s)`.

7) Replaces the contents with copies of the characters in the range
   `[``first``,``last``)`. This overload does not participate in overload
   resolution if `InputIt` does not satisfy LegacyInputIterator.(since C++11)

8) Replaces the contents with those of the initializer list `ilist`.

9) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then replaces the contents
   with those of `sv`, as if by `assign(sv.data(), sv.size())`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

10) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then replaces the contents
   with the characters from the subview `[``pos``,``pos + count``)` of `sv`. If
   the requested subview lasts past the end of `sv`, or if `count == npos`, the
   resulting subview is `[``pos``,``sv.size()``)`. If `pos > sv.size()`,
   `std::out_of_range` is thrown.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

### Parameters

- **count** — size of the resulting string
- **pos** — index of the first character to take
- **ch** — value to initialize characters of the string with
- **first, last** — range to copy the characters from
- **str** — string to be used as source to initialize the characters with
- **s** — pointer to a character string to use as source to initialize the
  string with
- **ilist** — `std::initializer_list` to initialize the characters of the string
  with
- **t** — object (convertible to `std::basic_string_view`) to initialize the
  characters of the string with

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Return value

`*this`

### Complexity

1) Linear in `count`.

2) Linear in size of `str`.

3) Linear in `count`.

4) Constant. If `alloc` is given and `alloc != other.get_allocator()`, then
   linear.

5) Linear in `count`.

6) Linear in size of `s`.

7) Linear in distance between `first` and `last`.

8) Linear in size of `ilist`.

9) Linear in size of `t`.

### Exceptions

4) `noexcept` specification: `noexcept(std::allocator_traits<Allocator>::
   propagate_on_container_move_assignment::value || std::allocator_traits
   <Allocator>::is_always_equal::value)`

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string s;
    // assign(size_type count, CharT ch)
    s.assign(4, '=');
    std::cout << s << '\n'; // "===="

    std::string const c("Exemplary");
    // assign(const basic_string& str)
    s.assign(c);
    std::cout << c << " == " << s << '\n'; // "Exemplary == Exemplary"

    // assign(const basic_string& str, size_type pos, size_type count)
    s.assign(c, 0, c.length() - 1);
    std::cout << s << '\n'; // "Exemplar";

    // assign(basic_string&& str)
    s.assign(std::string("C++ by ") + "example");
    std::cout << s << '\n'; // "C++ by example"

    // assign(const CharT* s, size_type count)
    s.assign("C-style string", 7);
    std::cout << s << '\n'; // "C-style"

    // assign(const CharT* s)
    s.assign("C-style\0string");
    std::cout << s << '\n'; // "C-style"

    char mutable_c_str[] = "C-style string";
    // assign(InputIt first, InputIt last)
    s.assign(std::begin(mutable_c_str), std::end(mutable_c_str) - 1);
    std::cout << s << '\n'; // "C-style string"

    // assign(std::initializer_list<CharT> ilist)
    s.assign({'C', '-', 's', 't', 'y', 'l', 'e'});
    std::cout << s << '\n'; // "C-style"
}
```

Output:

```text
====
Exemplary == Exemplary
Exemplar
C++ by example
C-style
C-style
C-style string
C-style
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2063 | C++11 | non-normative note stated that swap is a valid
      implementation of move-assign | corrected to require move assignment
  LWG 2579 | C++11 | assign(const basic_string&) did not propagate allocators |
      made to propagate allocators if needed
  LWG 2946 | C++17 | overload (9) caused ambiguity in some cases | avoided by
      making it a template

### See also

- **assign_range (C++23)** — assign a range of characters to a string (public
  member function)
- **(constructor)** — constructs a `basic_string` (public member function)
- **operator=** — assigns values to the string (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/assign*
