# operator==,!=,<,<=,>,>=,<=>(std::sub_match)

```cpp
Direct comparison
template< class BidirIt >
bool operator==( const std::sub_match<BidirIt>& lhs,
                 const std::sub_match<BidirIt>& rhs );  // (1) (since C++11)
template< class BidirIt >
bool operator!=( const std::sub_match<BidirIt>& lhs,
                 const std::sub_match<BidirIt>& rhs );  // (2) (since C++11) (until C++20)
template< class BidirIt >
bool operator<( const std::sub_match<BidirIt>& lhs,
                const std::sub_match<BidirIt>& rhs );  // (3) (since C++11) (until C++20)
template< class BidirIt >
bool operator<=( const std::sub_match<BidirIt>& lhs,
                 const std::sub_match<BidirIt>& rhs );  // (4) (since C++11) (until C++20)
template< class BidirIt >
bool operator>( const std::sub_match<BidirIt>& lhs,
                const std::sub_match<BidirIt>& rhs );  // (5) (since C++11) (until C++20)
template< class BidirIt >
bool operator>=( const std::sub_match<BidirIt>& lhs,
                 const std::sub_match<BidirIt>& rhs );  // (6) (since C++11) (until C++20)
template< class BidirIt >
/*comp-cat*/ operator<=>( const std::sub_match<BidirIt>& lhs,
                          const std::sub_match<BidirIt>& rhs );  // (7) (since C++20)
std::sub_match and std::basic_string
template< class BidirIt, class Traits, class Alloc >
bool operator==( const std::sub_match<BidirIt>& sm,
                 const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& rhs );  // (8) (since C++11)
template< class BidirIt, class Traits, class Alloc >
bool operator!=( const std::sub_match<BidirIt>& sm,
                 const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st );  // (9) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator<( const std::sub_match<BidirIt>& sm,
                const std::basic_string<
                    typename std::iterator_traits<BidirIt>::value_type,
                    Traits,Alloc>& st );  // (10) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator<=( const std::sub_match<BidirIt>& sm,
                 const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st );  // (11) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator>( const std::sub_match<BidirIt>& sm,
                const std::basic_string<
                    typename std::iterator_traits<BidirIt>::value_type,
                    Traits,Alloc>& st );  // (12) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator>=( const std::sub_match<BidirIt>& sm,
                 const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st );  // (13) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
/*comp-cat*/ operator<=>( const std::sub_match<BidirIt>& sm,
                          const std::basic_string<
                              typename std::iterator_traits<BidirIt>::value_type,
                              Traits,Alloc>& st );  // (14) (since C++20)
std::basic_string and std::sub_match
template< class BidirIt, class Traits, class Alloc >
bool operator==( const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st,
                 const std::sub_match<BidirIt>& sm );  // (15) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator!=( const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st,
                 const std::sub_match<BidirIt>& sm );  // (16) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator<( const std::basic_string<
                    typename std::iterator_traits<BidirIt>::value_type,
                    Traits,Alloc>& st,
                const std::sub_match<BidirIt>& sm );  // (17) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator<=( const std::basic_string<
                    typename std::iterator_traits<BidirIt>::value_type,
                    Traits,Alloc>& st,
                const std::sub_match<BidirIt>& sm );  // (18) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator>( const std::basic_string<
                    typename std::iterator_traits<BidirIt>::value_type,
                    Traits,Alloc>& st,
                const std::sub_match<BidirIt>& sm );  // (19) (since C++11) (until C++20)
template< class BidirIt, class Traits, class Alloc >
bool operator>=( const std::basic_string<
                     typename std::iterator_traits<BidirIt>::value_type,
                     Traits,Alloc>& st,
                 const std::sub_match<BidirIt>& sm );  // (20) (since C++11) (until C++20)
std::sub_match and std::iterator_traits<BidirIt>::value_type*
template< class BidirIt >
bool operator==( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type* s );  // (21) (since C++11)
template< class BidirIt >
bool operator!=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type* s );  // (22) (since C++11) (until C++20)
template< class BidirIt >
bool operator<( const std::sub_match<BidirIt>& sm,
                const typename std::iterator_traits<BidirIt>::value_type* s );  // (23) (since C++11) (until C++20)
template< class BidirIt >
bool operator<=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type* s );  // (24) (since C++11) (until C++20)
template< class BidirIt >
bool operator>( const std::sub_match<BidirIt>& sm,
                const typename std::iterator_traits<BidirIt>::value_type* s );  // (25) (since C++11) (until C++20)
template< class BidirIt >
bool operator>=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type* s );  // (26) (since C++11) (until C++20)
template< class BidirIt >
/*comp-cat*/ operator<=>( const std::sub_match<BidirIt>& sm,
                          const typename std::iterator_traits<BidirIt>::value_type* s );  // (27) (since C++20)
std::iterator_traits<BidirIt>::value_type* and std::sub_match
template< class BidirIt >
bool operator==( const typename std::iterator_traits<BidirIt>::value_type* s,
                 const std::sub_match<BidirIt>& sm );  // (28) (since C++11) (until C++20)
template< class BidirIt >
bool operator!=( const typename std::iterator_traits<BidirIt>::value_type* s,
                 const std::sub_match<BidirIt>& sm );  // (29) (since C++11) (until C++20)
template< class BidirIt >
bool operator<( const typename std::iterator_traits<BidirIt>::value_type* s,
                const std::sub_match<BidirIt>& sm );  // (30) (since C++11) (until C++20)
template< class BidirIt >
bool operator<=( const typename std::iterator_traits<BidirIt>::value_type* s,
                 const std::sub_match<BidirIt>& sm );  // (31) (since C++11) (until C++20)
template< class BidirIt >
bool operator>( const typename std::iterator_traits<BidirIt>::value_type* s,
                const std::sub_match<BidirIt>& sm );  // (32) (since C++11) (until C++20)
template< class BidirIt >
bool operator>=( const typename std::iterator_traits<BidirIt>::value_type* s,
                 const std::sub_match<BidirIt>& sm );  // (33) (since C++11) (until C++20)
std::sub_match and std::iterator_traits<BidirIt>::value_type
template< class BidirIt >
bool operator==( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type& ch );  // (34) (since C++11)
template< class BidirIt >
bool operator!=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type& ch );  // (35) (since C++11) (until C++20)
template< class BidirIt >
bool operator<( const std::sub_match<BidirIt>& sm,
                const typename std::iterator_traits<BidirIt>::value_type& ch );  // (36) (since C++11) (until C++20)
template< class BidirIt >
bool operator<=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type& ch );  // (37) (since C++11) (until C++20)
template< class BidirIt >
bool operator>( const std::sub_match<BidirIt>& sm,
                const typename std::iterator_traits<BidirIt>::value_type& ch );  // (38) (since C++11) (until C++20)
template< class BidirIt >
bool operator>=( const std::sub_match<BidirIt>& sm,
                 const typename std::iterator_traits<BidirIt>::value_type& ch );  // (39) (since C++11) (until C++20)
template< class BidirIt >
/*comp-cat*/ operator<=>( const std::sub_match<BidirIt>& sm,
                          const typename std::iterator_traits<BidirIt>::value_type& ch );  // (40) (since C++20)
std::iterator_traits<BidirIt>::value_type and std::sub_match
template< class BidirIt >
bool operator==( const typename std::iterator_traits<BidirIt>::value_type& ch,
                 const std::sub_match<BidirIt>& sm );  // (41) (since C++11) (until C++20)
template< class BidirIt >
bool operator!=( const typename std::iterator_traits<BidirIt>::value_type& ch,
                 const std::sub_match<BidirIt>& sm );  // (42) (since C++11) (until C++20)
template< class BidirIt >
bool operator<( const typename std::iterator_traits<BidirIt>::value_type& ch,
                const std::sub_match<BidirIt>& sm );  // (43) (since C++11) (until C++20)
template< class BidirIt >
bool operator<=( const typename std::iterator_traits<BidirIt>::value_type& ch,
                 const std::sub_match<BidirIt>& sm );  // (44) (since C++11) (until C++20)
template< class BidirIt >
bool operator>( const typename std::iterator_traits<BidirIt>::value_type& ch,
                const std::sub_match<BidirIt>& sm );  // (45) (since C++11) (until C++20)
template< class BidirIt >
bool operator>=( const typename std::iterator_traits<BidirIt>::value_type& ch,
                 const std::sub_match<BidirIt>& sm );  // (46) (since C++11) (until C++20)
```

Compares a `sub_match` to another `sub_match`, a string, a null-terminated
character sequence or a character.

1-7) Compares two `sub_match` directly by comparing their underlying character
   sequences. Implemented as if by `lhs.compare(rhs)`.

8-20) Compares a `sub_match` with a `std::basic_string`. Implemented as if by
   `sm.compare(typename sub_match<BidirIt>::string_type(st.data(), st.size())`.

21-33) Compares a `sub_match` with a null-terminated string. Implemented as if
   by `sm.compare(s)`.

34-46) Compares a `sub_match` with a character. Implemented as if by
   `sm.compare(typename sub_match<BidirIt>::string_type(1, ch))`.

The return type of three-way comparison operators (`/*comp-cat*/`) is
`std::compare_three_way_result_t<typename
std::sub_match<BidirIt>::string_type>>`, i.e. `std::char_traits<typename
std::iterator_traits<BidirIt>::value_type>::comparison_category` if that
qualified-id is valid and denotes a type, `std::weak_ordering` otherwise. If
`/*comp-cat*/` is not a comparison category type, the program is ill-formed.
The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

### Parameters

- **lhs, rhs, sm** — a `sub_match` to compare
- **st** — a `basic_string` to compare
- **s** — a pointer to a null-terminated string to compare
- **ch** — a character to compare

### Return value

For `operator==`, (since C++20)`true` if the corresponding comparison holds as
defined by `std::sub_match::compare()`, `false` otherwise.

For `operator<=>`, `static_cast</*comp-cat*/>(/*comp-res*/ <=> 0)`, where
`/*comp-res*/` is the result of `std::sub_match::compare()` described above.
*(since C++20)*

### Notes

If value type of `BidirIt` is `char`, `wchar_t`, `char8_t`, `char16_t`, or
`char32_t`, the return type of `operator<=>` is `std::strong_ordering`.
*(since C++20)*

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3432 | C++20 | the return type of `operator<=>` was not required to be a
      comparison category type | required

### See also

- **compare** — compares matched subsequence (if any) (public member function)

---
*Source: https://en.cppreference.com/w/cpp/regex/sub_match/operator_cmp*
