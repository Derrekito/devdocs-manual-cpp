# std::basic_string<CharT,Traits,Allocator>::basic_string

```cpp
basic_string();
explicit basic_string( const Allocator& alloc );  // (until C++17)
basic_string() noexcept(noexcept( Allocator() )) :
    basic_string( Allocator() ) {}
explicit basic_string( const Allocator& alloc ) noexcept;  // (since C++17) (until C++20)
constexpr basic_string() noexcept(noexcept( Allocator() )) :
    basic_string( Allocator() ) {}
constexpr explicit basic_string( const Allocator& alloc ) noexcept;  // (since C++20)
basic_string( size_type count, CharT ch,
              const Allocator& alloc = Allocator() );  // (until C++20)
constexpr basic_string( size_type count, CharT ch,
                        const Allocator& alloc = Allocator() );  // (since C++20)
basic_string( const basic_string& other, size_type pos,
              const Allocator& alloc = Allocator() );  // (until C++20)
constexpr basic_string( const basic_string& other, size_type pos,
                        const Allocator& alloc = Allocator() );  // (since C++20)
constexpr basic_string( basic_string&& other, size_type pos,
                        const Allocator& alloc = Allocator() );  // (3) (since C++23)
basic_string( const basic_string& other,
              size_type pos, size_type count,
              const Allocator& alloc = Allocator() );  // (until C++20)
constexpr basic_string( const basic_string& other,
                        size_type pos, size_type count,
                        const Allocator& alloc = Allocator() );  // (since C++20)
constexpr basic_string( basic_string&& other,
                        size_type pos, size_type count,
                        const Allocator& alloc = Allocator() );  // (3) (since C++23)
basic_string( const CharT* s, size_type count,
              const Allocator& alloc = Allocator() );  // (until C++20)
constexpr basic_string( const CharT* s, size_type count,
                        const Allocator& alloc = Allocator() );  // (since C++20)
basic_string( const CharT* s, const Allocator& alloc = Allocator() );  // (until C++20)
constexpr basic_string( const CharT* s,
                        const Allocator& alloc = Allocator() );  // (since C++20)
template< class InputIt >
basic_string( InputIt first, InputIt last,
              const Allocator& alloc = Allocator() );  // (until C++20)
template< class InputIt >
constexpr basic_string( InputIt first, InputIt last,
                        const Allocator& alloc = Allocator() );  // (since C++20)
basic_string( const basic_string& other );  // (until C++20)
constexpr basic_string( const basic_string& other );  // (since C++20)
basic_string( const basic_string& other, const Allocator& alloc );  // (since C++11) (until C++20)
constexpr basic_string( const basic_string& other, const Allocator& alloc );  // (since C++20)
basic_string( basic_string&& other ) noexcept;  // (since C++11) (until C++20)
constexpr basic_string( basic_string&& other ) noexcept;  // (since C++20)
basic_string( basic_string&& other, const Allocator& alloc );  // (since C++11) (until C++20)
constexpr basic_string( basic_string&& other, const Allocator& alloc );  // (since C++20)
basic_string( std::initializer_list<CharT> ilist,
              const Allocator& alloc = Allocator() );  // (since C++11) (until C++20)
constexpr basic_string( std::initializer_list<CharT> ilist,
                        const Allocator& alloc = Allocator() );  // (since C++20)
template< class StringViewLike >
explicit basic_string( const StringViewLike& t,
                       const Allocator& alloc = Allocator() );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr explicit basic_string( const StringViewLike& t,
                                 const Allocator& alloc = Allocator() );  // (since C++20)
template< class StringViewLike >
basic_string( const StringViewLike& t, size_type pos, size_type n,
              const Allocator& alloc = Allocator() );  // (since C++17) (until C++20)
template< class StringViewLike >
constexpr basic_string( const StringViewLike& t, size_type pos, size_type n,
                        const Allocator& alloc = Allocator() );  // (since C++20)
basic_string( std::nullptr_t ) = delete;  // (12) (since C++23)
template< container-compatible-range<CharT> R >
constexpr basic_string( std::from_range_t, R&& rg,
                        const Allocator& = Allocator());  // (13) (since C++23)
```

Constructs new string from a variety of data sources and optionally using user
supplied allocator `alloc`.

1) Default constructor. Constructs empty string (zero size and unspecified
   capacity). If no allocator is supplied, allocator is obtained from a
   default-constructed instance.

2) Constructs the string with `count` copies of character `ch`. This constructor
   is not used for class template argument deduction if the `Allocator` type
   that would be deduced does not qualify as an allocator. (since C++17)

3) Constructs the string with a substring `[``pos``,``pos + count``)` of
   `other`. If `count == npos`, if `count` is not specified, or if the requested
   substring lasts past the end of the string, the resulting substring is
   `[``pos``,``other.size()``)`. If `other` is an rvalue reference, it is left
   in a valid but unspecified state.(since C++23)

4) Constructs the string with the first `count` characters of character string
   pointed to by `s`. `s` can contain null characters. The length of the string
   is `count`. The behavior is undefined if `[``s``,``s + count``)` is not a
   valid range.

5) Constructs the string with the contents initialized with a copy of the
   null-terminated character string pointed to by `s`. The length of the string
   is determined by the first null character. The behavior is undefined if
   `[``s``,``s + Traits::length(s)``)` is not a valid range (for example, if `s`
   is a null pointer). This constructor is not used for class template argument
   deduction if the `Allocator` type that would be deduced does not qualify as
   an allocator. (since C++17)

6) Constructs the string with the contents of the range `[``first``,``last``)`.
   If `InputIt` is an integral type, equivalent to overload (2), as if by
   `basic_string(static_cast<size_type>(first), static_cast<value_type>(last),
   alloc)`. (until C++11) This constructor only participates in overload
   resolution if `InputIt` satisfies LegacyInputIterator. (since C++11)

7) Copy constructor. Constructs the string with a copy of the contents of
   `other`.

8) Move constructor. Constructs the string with the contents of `other` using
   move semantics. `other` is left in valid, but unspecified state.

9) Constructs the string with the contents of the initializer list `ilist`.

10) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then initializes the string
   with the contents of `sv`, as if by `basic_string(sv.data(), sv.size(),
   alloc)`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

11) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then initializes the string
   with the subrange `[``pos``,``pos + n``)` of `sv` as if by
   `basic_string(sv.substr(pos, n), alloc)`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true`.

12) `std::basic_string` cannot be constructed from `nullptr`.

13) Constructs the string with the values contained in the range `rg`.

### Parameters

- **alloc** — allocator to use for all memory allocations of this string
- **count** — size of the resulting string
- **ch** — value to initialize the string with
- **pos** — position of the first character to include
- **first, last** — range to copy the characters from
- **s** — pointer to an array of characters to use as source to initialize the
  string with
- **other** — another string to use as source to initialize the string with
- **ilist** — `std::initializer_list` to initialize the string with
- **t** — object (convertible to `std::basic_string_view`) to initialize the
  string with
- **rg** — a container compatible range

### Complexity

1) Constant.

2-4) Linear in `count`.

5) Linear in length of `s`.

6) Linear in distance between `first` and `last`.

7) Linear in size of `other`.

8) Constant. If `alloc` is given and `alloc != other.get_allocator()`, then
   linear.

9) Linear in size of `ilist`.

13) Linear in size of `rg`.

### Exceptions

3) `std::out_of_range` if `pos > other.size()`.

8) Throws nothing if `alloc == str.get_allocator()`.

11) `std::out_of_range` if `pos` is out of range.

Throws `std::length_error` if the length of the constructed string would exceed
`max_size()` (for example, if `count > max_size()` for (2)). Calls to
`Allocator::allocate` may throw.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Notes

Initialization with a string literal that contains embedded `'\0'` characters
uses the overload (5), which stops at the first null character. This can be
avoided by specifying a different constructor or by using `operator""s`:

```cpp
std::string s1 = "ab\0\0cd";   // s1 contains "ab"
std::string s2{"ab\0\0cd", 6}; // s2 contains "ab\0\0cd"
std::string s3 = "ab\0\0cd"s;  // s3 contains "ab\0\0cd"
```

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Tagged constructor (13) to
      construct from container compatible range

### Example

```cpp
#include <cassert>
#include <cctype>
#include <iomanip>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::cout << "1) string(); ";
    std::string s1;
    assert(s1.empty() && (s1.length() == 0) && (s1.size() == 0));
    std::cout << "s1.capacity(): " << s1.capacity() << '\n'; // unspecified

    std::cout << "2) string(size_type count, CharT ch): ";
    std::string s2(4, '=');
    std::cout << std::quoted(s2) << '\n'; // "===="

    std::cout << "3) string(const string& other, size_type pos, size_type count): ";
    std::string const other3("Exemplary");
    std::string s3(other3, 0, other3.length() - 1);
    std::cout << std::quoted(s3) << '\n'; // "Exemplar"

    std::cout << "4) string(const string& other, size_type pos): ";
    std::string const other4("Mutatis Mutandis");
    std::string s4(other4, 8);
    std::cout << std::quoted(s4) << '\n'; // "Mutandis", i.e. [8, 16)

    std::cout << "5) string(CharT const* s, size_type count): ";
    std::string s5("C-style string", 7);
    std::cout << std::quoted(s5) << '\n'; // "C-style", i.e. [0, 7)

    std::cout << "6) string(CharT const* s): ";
    std::string s6("C-style\0string");
    std::cout << std::quoted(s6) << '\n'; // "C-style"

    std::cout << "7) string(InputIt first, InputIt last): ";
    char mutable_c_str[] = "another C-style string";
    std::string s7(std::begin(mutable_c_str) + 8, std::end(mutable_c_str) - 1);
    std::cout << std::quoted(s7) << '\n'; // "C-style string"

    std::cout << "8) string(string&): ";
    std::string const other8("Exemplar");
    std::string s8(other8);
    std::cout << std::quoted(s8) << '\n'; // "Exemplar"

    std::cout << "9) string(string&&): ";
    std::string s9(std::string("C++ by ") + std::string("example"));
    std::cout << std::quoted(s9) << '\n'; // "C++ by example"

    std::cout << "a) string(std::initializer_list<CharT>): ";
    std::string sa({'C', '-', 's', 't', 'y', 'l', 'e'});
    std::cout << std::quoted(sa) << '\n'; // "C-style"

    // before C++11, overload resolution selects string(InputIt first, InputIt last)
    // [with InputIt = int] which behaves *as if* string(size_type count, CharT ch)
    // after C++11 the InputIt constructor is disabled for integral types and calls:
    std::cout << "b) string(size_type count, CharT ch) is called: ";
    std::string sb(3, std::toupper('a'));
    std::cout << std::quoted(sb) << '\n'; // "AAA"

//  std::string sc(nullptr); // Before C++23: throws std::logic_error
                             // Since C++23: won't compile, see overload (12)
//  std::string sc(0); // Same as above, as literal 0 is a null pointer constant

    auto const range = {0x43, 43, 43};
#ifdef __cpp_lib_containers_ranges
    std::string sc(std::from_range, range); // tagged constructor (13)
    std::cout << "c) string(std::from_range, range) is called: ";
#else
    std::string sc(range.begin(), range.end()); // fallback to overload (7)
    std::cout << "c) string(range.begin(), range.end()) is called: ";
#endif
    std::cout << std::quoted(sc) << '\n'; // "C++"
}
```

Possible output:

```text
1) string(); s1.capacity(): 15
2) string(size_type count, CharT ch): "===="
3) string(const string& other, size_type pos, size_type count): "Exemplar"
4) string(const string& other, size_type pos): "Mutandis"
5) string(CharT const* s, size_type count): "C-style"
6) string(CharT const* s): "C-style"
7) string(InputIt first, InputIt last): "C-style string"
8) string(string&): "Exemplar"
9) string(string&&): "C++ by example"
a) string(std::initializer_list<CharT>): "C-style"
b) string(size_type count, CharT ch) is called: "AAA"
c) string(std::from_range, range) is called: "C++"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 301 | C++98 | overload (6) did not use the parameter `alloc` if `InputIt`
      is an integral type | use that parameter
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2193 | C++11 | the default constructor is explicit | made non-explicit
  LWG 2583 | C++98 | there is no way to supply an allocator for
      `basic_string(str, pos)` | there is a constructor for `basic_string(str,
      pos, alloc)`
  LWG 2946 | C++17 | overload (10) causes ambiguity in some cases | avoided by
      making it a template
  LWG 3076 | C++17 | two constructors may cause ambiguities in class template
      argument deduction | constrained

### See also

- **assign** — assign characters to a string (public member function)
- **operator=** — assigns values to the string (public member function)
- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)
- **to_wstring (C++11)** — converts an integral or floating-point value to
  `wstring` (function)
- **(constructor)** — constructs a `basic_string_view` (public member function
  of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/basic_string*
