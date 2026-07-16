# operator==,!=,<,<=,>,>=,<=>(std::basic_string)

```cpp
Compare two basic_string objects
template< class CharT, class Traits, class Alloc >
bool operator==( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator==( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
constexpr bool
    operator==( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++20)
template< class CharT, class Traits, class Alloc >
bool operator!=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator!=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator<( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator<=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator>( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++11)
template< class CharT, class Traits, class Alloc >
bool operator>=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (since C++11) (until C++20)
template< class CharT, class Traits, class Alloc >
constexpr /*comp-cat*/
    operator<=>( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs ) noexcept;  // (7) (since C++20)
Compare a basic_string object and null-terminated array of T
template< class CharT, class Traits, class Alloc >
bool operator==( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
constexpr bool operator==( const std::basic_string<CharT,Traits,Alloc>& lhs,
                           const CharT* rhs );  // (since C++20)
template< class CharT, class Traits, class Alloc >
bool operator==( const CharT* lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator!=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator!=( const CharT* lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<( const CharT* lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator<=( const CharT* lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>( const std::basic_string<CharT,Traits,Alloc>& lhs,
                const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>( const CharT* lhs,
                const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>=( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const CharT* rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
bool operator>=( const CharT* lhs,
                 const std::basic_string<CharT,Traits,Alloc>& rhs );  // (until C++20)
template< class CharT, class Traits, class Alloc >
constexpr /*comp-cat*/
    operator<=>( const std::basic_string<CharT,Traits,Alloc>& lhs,
                 const CharT* rhs );  // (14) (since C++20)
```

Compares the contents of a string with another string or a null-terminated array
of `CharT`.

All comparisons are done via the `compare()` member function (which itself is
defined in terms of `Traits::compare()`):

- Two strings are equal if both the size of `lhs` and `rhs` are equal and each
  character in `lhs` has equivalent character in `rhs` at the same position.
- The ordering comparisons are done lexicographically -- the comparison is
  performed by a function equivalent to `std::lexicographical_compare` or
  `std::lexicographical_compare_three_way`(since C++20).

1-7) Compares two `basic_string` objects.

8-14) Compares a `basic_string` object and a null-terminated array of `CharT`.

The return type of three-way comparison operators (`/*comp-cat*/`) is
`Traits::comparison_category` if that qualified-id exists and denotes a type,
`std::weak_ordering` otherwise. If `/*comp-cat*/` is not a comparison category
type, the program is ill-formed.
The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs** — strings whose contents to compare

### Return value

1-6,8-13) `true` if the corresponding comparison holds, `false` otherwise.

7,14) `static_cast</*comp-cat*/>(lhs.compare(rhs) <=> 0)`.

### Complexity

Linear in the size of the strings.

### Notes

If at least one parameter is of type `std::string`, `std::wstring`,
`std::u8string`, `std::u16string`, or `std::u32string`, the return type of
`operator<=>` is `std::strong_ordering`.
*(since C++20)*

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2064 | C++11 | whether overloads taking two `basic_string`s are noexcept
      was inconsistent; overloads taking a `CharT*` were noexcept but might
      raise UB | made consistent; noexcept removed
  LWG 3432 | C++20 | the return type of `operator<=>` was not required to be a
      comparison category type | required

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/operator_cmp*
