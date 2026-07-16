# Standard library header <bitset>

This header is part of the general utility library.

**Includes**

- **<string>** — `std::basic_string` class template
- **<iosfwd>** — Forward declarations of all classes in the input/output library

**Classes**

- **bitset** — implements constant length bit array (class template)
- **std::hash<std::bitset> (C++11)** — hash support for `std::bitset` (class
  template specialization)

**Forward declarations**

- **hash (C++11)** — hash function object (class template)

**Functions**

- **operator&operator|operator^** — performs binary logic operations on bitsets
  (function template)
- **operator<<operator>>** — performs stream input and output of bitsets
  (function template)

### Synopsis

```cpp
#include <string>
#include <iosfwd>   // for istream, ostream

namespace std {
  template<size_t N> class bitset;

  // bitset operators
  template<size_t N>
    constexpr bitset<N> operator&(const bitset<N>&, const bitset<N>&) noexcept;
  template<size_t N>
    constexpr bitset<N> operator|(const bitset<N>&, const bitset<N>&) noexcept;
  template<size_t N>
    constexpr bitset<N> operator^(const bitset<N>&, const bitset<N>&) noexcept;
  template<class CharT, class Traits, size_t N>
    basic_istream<CharT, Traits>&
      operator>>(basic_istream<CharT, Traits>& is, bitset<N>& x);
  template<class CharT, class Traits, size_t N>
    basic_ostream<CharT, Traits>&
      operator<<(basic_ostream<CharT, Traits>& os, const bitset<N>& x);
}
```

#### Class template `std::bitset`

```cpp
namespace std {
  template<size_t N> class bitset {
  public:
    // bit reference
    class reference {
      friend class bitset;
      constexpr reference() noexcept;

    public:
      constexpr reference(const reference&) = default;
      constexpr ~reference();
      constexpr reference& operator=(bool x) noexcept;            // for b[i] = x;
      constexpr reference& operator=(const reference&) noexcept;  // for b[i] = b[j];
      constexpr bool operator~() const noexcept;                  // flips the bit
      constexpr operator bool() const noexcept;                   // for x = b[i];
      constexpr reference& flip() noexcept;                       // for b[i].flip();
    };

    // constructors
    constexpr bitset() noexcept;
    constexpr bitset(unsigned long long val) noexcept;
    template<class CharT, class Traits, class Allocator>
      constexpr explicit bitset(
        const basic_string<CharT, Traits, Allocator>& str,
        typename basic_string<CharT, Traits, Allocator>::size_type pos = 0,
        typename basic_string<CharT, Traits, Allocator>::size_type n
          = basic_string<CharT, Traits, Allocator>::npos,
        CharT zero = CharT('0'),
        CharT one = CharT('1'));
    template<class CharT>
      constexpr explicit bitset(
        const CharT* str,
        typename basic_string<CharT>::size_type n = basic_string<CharT>::npos,
        CharT zero = CharT('0'),
        CharT one = CharT('1'));

    // bitset operations
    constexpr bitset& operator&=(const bitset& rhs) noexcept;
    constexpr bitset& operator|=(const bitset& rhs) noexcept;
    constexpr bitset& operator^=(const bitset& rhs) noexcept;
    constexpr bitset& operator<<=(size_t pos) noexcept;
    constexpr bitset& operator>>=(size_t pos) noexcept;
    constexpr bitset& set() noexcept;
    constexpr bitset& set(size_t pos, bool val = true);
    constexpr bitset& reset() noexcept;
    constexpr bitset& reset(size_t pos);
    constexpr bitset  operator~() const noexcept;
    constexpr bitset& flip() noexcept;
    constexpr bitset& flip(size_t pos);

    // element access
    constexpr bool operator[](size_t pos) const;        // for b[i];
    constexpr reference operator[](size_t pos);         // for b[i];

    constexpr unsigned long to_ulong() const;
    constexpr unsigned long long to_ullong() const;
    template<class CharT = char,
             class Traits = char_Traits<CharT>,
             class Allocator = allocator<CharT>>
      constexpr basic_string<CharT, Traits, Allocator>
        to_string(CharT zero = CharT('0'), CharT one = CharT('1')) const;

    constexpr size_t count() const noexcept;
    constexpr size_t size() const noexcept;
    constexpr bool operator==(const bitset& rhs) const noexcept;
    constexpr bool test(size_t pos) const;
    constexpr bool all() const noexcept;
    constexpr bool any() const noexcept;
    constexpr bool none() const noexcept;
    constexpr bitset operator<<(size_t pos) const noexcept;
    constexpr bitset operator>>(size_t pos) const noexcept;
  };

  // hash support
  template<class T> struct hash;
  template<size_t N> struct hash<bitset<N>>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/bitset*
