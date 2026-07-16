# operator==,!=,<,<=,>,>=,<=>(std::basic_string_view)

```cpp
template< class CharT, class Traits >
constexpr bool operator==( std::basic_string_view<CharT,Traits> lhs,
                           std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (1) (since C++17)
template< class CharT, class Traits >
constexpr bool operator!=( std::basic_string_view<CharT,Traits> lhs,
                           std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (2) (since C++17) (until C++20)
template< class CharT, class Traits >
constexpr bool operator<( std::basic_string_view<CharT,Traits> lhs,
                          std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (3) (since C++17) (until C++20)
template< class CharT, class Traits >
constexpr bool operator<=( std::basic_string_view<CharT,Traits> lhs,
                           std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (4) (since C++17) (until C++20)
template< class CharT, class Traits >
constexpr bool operator>( std::basic_string_view<CharT,Traits> lhs,
                          std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (5) (since C++17) (until C++20)
template< class CharT, class Traits >
constexpr bool operator>=( std::basic_string_view<CharT,Traits> lhs,
                           std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (6) (since C++17) (until C++20)
template< class CharT, class Traits >
constexpr /*comp-cat*/
    operator<=>( std::basic_string_view<CharT,Traits> lhs,
                 std::basic_string_view<CharT,Traits> rhs ) noexcept;  // (7) (since C++20)
```

Compares two views.

All comparisons are done via the `compare()` member function (which itself is
defined in terms of `Traits::compare()`):

- Two views are equal if both the size of `lhs` and `rhs` are equal and each
  character in `lhs` has an equivalent character in `rhs` at the same position.
- The ordering comparisons are done lexicographically -- the comparison is
  performed by a function equivalent to `std::lexicographical_compare`.

The return type of three-way comparison operators (`/*comp-cat*/`) is
`Traits::comparison_category` if that qualified-id denotes a type,
`std::weak_ordering` otherwise. If `/*comp-cat*/` is not a comparison category
type, the program is ill-formed.
The `<`, `<=`, `>`, `>=`, and `!=` operators are synthesized from operator<=>
and operator== respectively.
*(since C++20)*

The implementation shall provide sufficient additional `constexpr` and
`noexcept` overloads of these functions so that a
`basic_string_view<CharT,Traits>` object `sv` may be compared to another object
`t` with an implicit conversion to `basic_string_view<CharT,Traits>`, with
semantics identical to comparing `sv` and `basic_string_view<CharT,Traits>(t)`.

### Parameters

- **lhs, rhs** — views to compare

### Return value

1-6) `true` if the corresponding comparison holds, `false` otherwise.

7) `static_cast</*comp-cat*/>(lhs.compare(rhs) <=> 0)`.

### Complexity

Linear in the size of the views.

### Notes

Sufficient additional overloads can be implemented through non-deduced context
in one parameter type.

Three-way comparison result type of `std::string_view`, `std::wstring_view`,
`std::u8string_view`, `std::u16string_view` and `std::u32string_view` is
`std::strong_ordering`.
*(since C++20)*

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3432 | C++20 | the return type of `operator<=>` was not required to be a
      comparison category type | required

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view/operator_cmp*
