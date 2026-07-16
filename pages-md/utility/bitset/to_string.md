# std::bitset<N>::to_string

```cpp
template<
    class CharT,
    class Traits,
    class Allocator
>
std::basic_string<CharT, Traits, Allocator>
    to_string( CharT zero = CharT('0'), CharT one = CharT('1') ) const;  // (until C++11)
template<
    class CharT = char,
    class Traits = std::char_traits<CharT>,
    class Allocator = std::allocator<CharT>
>
std::basic_string<CharT, Traits, Allocator>
    to_string( CharT zero = CharT('0'), CharT one = CharT('1') ) const;  // (since C++11) (constexpr since C++23)
template< class CharT, class Traits >
std::basic_string<CharT, Traits>
    to_string( CharT zero = CharT('0'), CharT one = CharT('1') ) const;  // (2) (until C++11)
template< class CharT >
std::basic_string<CharT>
    to_string( CharT zero = CharT('0'), CharT one = CharT('1') ) const;  // (3) (until C++11)
std::string to_string( char zero = '0', char one = '1' ) const;  // (4) (until C++11)
```

Converts the contents of the bitset to a string. Uses `zero` to represent bits
with value of `false` and `one` to represent bits with value of `true`.

The resulting string contains `N` characters with the first character
corresponds to the last (`N-1`th) bit and the last character corresponding to
the first bit.

All template type arguments need to be provided because function templates
cannot have default template arguments. Overloads (2-4) are provided to simplify
the invocations of `to_string`:
2) Uses the default allocator `std::allocator`.
3) Uses the default character trait `std::char_traits` and the default allocator
`std::allocator`.
4) Uses the default character type char, the default character trait
`std::char_traits` and the default allocator `std::allocator`.
*(until C++11)*

### Parameters

- **zero** — character to use to represent `false`
- **one** — character to use to represent `true`

### Return value

1) The converted string.

2) `to_string<CharT, Traits, std::allocator<CharT>>(zero, one)`.

3) `to_string<CharT, std::char_traits<CharT>, std::allocator<CharT>>(zero,
   one)`.

4) `to_string<char, std::char_traits<char>, std::allocator<char>>(zero, one)`.

### Exceptions

May throw `std::bad_alloc` from the `std::string` constructor.

### Notes

Since C++11, functions templates can have default template arguments. LWG issue
1113 removed the helper overloads (2-4) and added the corresponding default
template arguments in (1).

### Example

```cpp
#include <bitset>
#include <iostream>

int main()
{
    std::bitset<8> b{42};
    std::cout << b.to_string() << '\n'
              << b.to_string('*') << '\n'
              << b.to_string('O', 'X') << '\n';
}
```

Output:

```text
00101010
**1*1*1*
OOXOXOXO
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 396 | C++98 | zero and one bits were converted to characters `​0​` and `1`
      (which do not correspond to `'0'` and `'1'`) | added parameters to provide
      values for these characters
  LWG 434 | C++98 | all template arguments needed to be provided | added
      overloads (2-4)
  LWG 853 | C++98 | overloads (2-4) did not have the default arguments added by
      LWG issue 396 | also added

### See also

- **to_ulong** — returns an unsigned long integer representation of the data
  (public member function)
- **to_ullong (C++11)** — returns an unsigned long long integer representation
  of the data (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/to_string*
