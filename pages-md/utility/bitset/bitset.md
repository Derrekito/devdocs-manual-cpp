# std::bitset<N>::bitset

```cpp
bitset();  // (until C++11)
constexpr bitset() noexcept;  // (since C++11)
bitset( unsigned long val );  // (until C++11)
constexpr bitset( unsigned long long val ) noexcept;  // (since C++11)
template< class CharT, class Traits, class Alloc >
explicit bitset( const std::basic_string<CharT, Traits, Alloc>& str,
                 typename std::basic_string<CharT, Traits, Alloc>::size_type
                     pos = 0,
                 typename std::basic_string<CharT, Traits, Alloc>::size_type
                     n = std::basic_string<CharT, Traits, Alloc>::npos,
                 CharT zero = CharT('0'),
                 CharT one = CharT('1') );  // (3) (constexpr since C++23)
template< class CharT, class Traits >
constexpr explicit bitset( std::basic_string_view<CharT, Traits> str,
                           std::size_t pos = 0,
                           std::size_t n = std::size_t(-1),
                           CharT zero = CharT('0'),
                           CharT one = CharT('1') );  // (4) (since C++26)
template< class CharT >
explicit bitset( const CharT* str,
                 std::size_t n = std::size_t(-1),
                 CharT zero = CharT('0'),
                 CharT one = CharT('1') );  // (5) (since C++11) (constexpr since C++23)
```

Constructs a new bitset from one of several optional data sources:

1) Default constructor. Constructs a bitset with all bits set to zero.
2) Constructs a bitset, initializing the first (rightmost, least significant)
`M` bit positions to the corresponding bit values of `val`, where `M` is the
smaller of

- `N`, and

- the number of bits in the value representation of unsigned long. For typical
  implementations, it is 32 or 64.
*(until C++11)*

- the number of bits in the value representation of unsigned long long. For
  typical implementations, it is 64.
*(since C++11)*

If `M` is less than `N`, the remaining bit positions are initialized to zeroes.

3) Constructs a bitset using the characters in the `std::basic_string` `str`. An
   optional starting position `pos` and length `n` can be provided, as well as
   characters denoting alternate values for set (`one`) and unset (`zero`) bits.
   `Traits::eq()` is used to compare the character values.

The effective length of the initializing string is `std::min(n, str.size() -
   pos)`.

If `pos > str.size()`, this constructor throws `std::out_of_range`. If any
   character examined in `str` is not `zero` or `one`, this constructor throws
   `std::invalid_argument`.

4) Similar to (3), but uses a `std::basic_string_view` instead of a
   `std::basic_string`.

5) Similar to (3), but uses a `const CharT*` instead of a `std::basic_string`.
   Equivalent to bitset(n == std::size_t(-1) ? basic_string<CharT>(str) :
   basic_string<CharT>(str, n), 0, n, zero, one) This constructor may
   dynamically allocate memory, although implementations generally avoid dynamic
   allocation. (until C++26) This constructor does not dynamically allocate
   memory, as if only `std::basic_string_view` is involved. (since C++26)

### Parameters

- **val** — number used to initialize the bitset
- **str** — string used to initialize the bitset
- **pos** — a starting offset into `str`
- **n** — number of characters to use from `str`
- **one** — alternate character for set bits in `str`
- **zero** — alternate character for unset bits in `str`

### Exceptions

3,4) `std::out_of_range` if `pos > str.size()`, `std::invalid_argument` if any
   character is not `one` or `zero`.

5) `std::invalid_argument` if any character is not `one` or `zero`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_bitset` | 202207L | (C++23) | A more constexpr
      `std::bitset`, overloads (3,5)
  `__cpp_lib_bitset` | 202306L | (C++26) | Interfacing `std::bitset` with
      `std::string_view`, (4)

### Example

```cpp
#include <bitset>
#include <climits>
#include <iostream>
#include <string>

int main()
{
    // empty constructor (1)
    std::bitset<8> b1; // [0,0,0,0,0,0,0,0]

    // unsigned long long constructor (2)
    std::bitset<8> b2(42);          // [0,0,1,0,1,0,1,0]
    std::bitset<70> bl(ULLONG_MAX); // [0,0,0,0,0,0,1,1,1,...,1,1,1] in C++11
    std::bitset<8> bs(0xfff0);      // [1,1,1,1,0,0,0,0]

    // string constructor (3)
    std::string bit_string = "110010";
    std::bitset<8> b3(bit_string);       // [0,0,1,1,0,0,1,0]
    std::bitset<8> b4(bit_string, 2);    // [0,0,0,0,0,0,1,0]
    std::bitset<8> b5(bit_string, 2, 3); // [0,0,0,0,0,0,0,1]

    // string constructor using custom zero/one digits (3)
    std::string alpha_bit_string = "aBaaBBaB";
    std::bitset<8> b6(alpha_bit_string, 0, alpha_bit_string.size(),
                      'a', 'B');         // [0,1,0,0,1,1,0,1]

    // char* constructor using custom digits (5)
    std::bitset<8> b7("XXXXYYYY", 8, 'X', 'Y'); // [0,0,0,0,1,1,1,1]

    std::cout <<   "b1: " << b1 << "\nb2: " << b2 << "\nbl: " << bl
              << "\nbs: " << bs << "\nb3: " << b3 << "\nb4: " << b4
              << "\nb5: " << b5 << "\nb6: " << b6 << "\nb7: " << b7 << '\n';
}
```

Possible output:

```text
b1: 00000000
b2: 00101010
bl: 0000001111111111111111111111111111111111111111111111111111111111111111
bs: 11110000
b3: 00110010
b4: 00000010
b5: 00000001
b6: 01001101
b7: 00001111
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 396 | C++98 | the values of the zero and one characters for overload (3)
      were `​0​` and `1` (which do not correspond to `'0'` and `'1'`) | added
      parameters to provide values for these characters
  LWG 457 | C++98 | `M` is the smaller of `N` and the value `CHAR_BIT *
      sizeof(unsigned long)` for overload (2), but unsigned long is not
      guaranteed to use all its bits to represent its value | consider the
      number of bits of the value representation instead

### See also

- **set** — sets bits to `true` or given value (public member function)
- **reset** — sets bits to `false` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/bitset/bitset*
