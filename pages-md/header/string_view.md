# Standard library header <string_view> (C++17)

This header is part of the strings library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **basic_string_view (C++17)** — read-only string view (class template)
- **`std::string_view` (C++17)** — std::basic_string_view<char>
- **`std::u8string_view` (C++20)** — std::basic_string_view<char8_t>
- **`std::u16string_view` (C++17)** — std::basic_string_view<char16_t>
- **`std::u32string_view` (C++17)** — std::basic_string_view<char32_t>
- **`std::wstring_view` (C++17)** — std::basic_string_view<wchar_t>
-
  **std::hash<std::string_view>std::hash<std::wstring_view>std::hash<std::u8string_view>std::hash<std::u16string_view>std::hash<std::u32string_view>
  (C++17)(C++17)(C++20)(C++17)(C++17)** — hash support for string views (class
  template specialization)

**Forward declarations**

- **hash (C++11)** — hash function object (class template)

**Functions**

- **operator==operator!=operator<operator>operator<=operator>=operator<=>
  (C++17)(removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares two string
  views (function template)
- **operator<< (C++17)** — performs stream output on string views (function
  template)
- **swap** — swaps the values of two objects (function template)

**Range access**

- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)
- **rbegincrbegin (C++14)** — returns a reverse iterator to the beginning of a
  container or array (function template)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)
- **sizessize (C++17)(C++20)** — returns the size of a container or array
  (function template)
- **empty (C++17)** — checks whether the container is empty (function template)
- **data (C++17)** — obtains the pointer to the underlying array (function
  template)

**Literals**

- **operator""sv (C++17)** — creates a string view of a character array literal
  (function)

### Synopsis

```cpp
#include <compare>

namespace std {
  // class template basic_string_view
  template<class CharT, class Traits = char_traits<CharT>>
  class basic_string_view;

  template<class CharT, class Traits>
    inline constexpr bool ranges::enable_view<basic_string_view<CharT, Traits>> = true;
  template<class CharT, class Traits>
    inline constexpr bool ranges::enable_borrowed_range<basic_string_view<CharT, Traits>> =
      true;

  // non-member comparison functions
  template<class CharT, class Traits>
    constexpr bool operator==(basic_string_view<CharT, Traits> x,
                              basic_string_view<CharT, Traits> y) noexcept;
  template<class CharT, class Traits>
    constexpr /* see description */
      operator<=>(basic_string_view<CharT, Traits> x,
                  basic_string_view<CharT, Traits> y) noexcept;

  // sufficient additional overloads of comparison functions

  // inserters and extractors
  template<class CharT, class Traits>
    basic_ostream<CharT, Traits>&
      operator<<(basic_ostream<CharT, Traits>& os,
                 basic_string_view<CharT, Traits> str);

  // basic_string_view typedef names
  using string_view    = basic_string_view<char>;
  using u8string_view  = basic_string_view<char8_t>;
  using u16string_view = basic_string_view<char16_t>;
  using u32string_view = basic_string_view<char32_t>;
  using wstring_view   = basic_string_view<wchar_t>;

  // hash support
  template<class T> struct hash;
  template<> struct hash<string_view>;
  template<> struct hash<u8string_view>;
  template<> struct hash<u16string_view>;
  template<> struct hash<u32string_view>;
  template<> struct hash<wstring_view>;

  inline namespace literals {
  inline namespace string_view_literals {
    // suffix for basic_string_view literals
    constexpr string_view    operator""sv(const char* str, size_t len) noexcept;
    constexpr u8string_view  operator""sv(const char8_t* str, size_t len) noexcept;
    constexpr u16string_view operator""sv(const char16_t* str, size_t len) noexcept;
    constexpr u32string_view operator""sv(const char32_t* str, size_t len) noexcept;
    constexpr wstring_view   operator""sv(const wchar_t* str, size_t len) noexcept;
  }
  }
}
```

#### Class template `std::basic_string_view`

```cpp
namespace std {
  template<class CharT, class Traits = char_traits<CharT>>
  class basic_string_view {
  public:
    // types
    using Traits_type            = Traits;
    using value_type             = CharT;
    using pointer                = value_type*;
    using const_pointer          = const value_type*;
    using reference              = value_type&;
    using const_reference        = const value_type&;
    using const_iterator         = /* implementation-defined */
    using iterator               = const_iterator;
    using const_reverse_iterator = reverse_iterator<const_iterator>;
    using reverse_iterator       = const_reverse_iterator;
    using size_type              = size_t;
    using difference_type        = ptrdiff_t;
    static constexpr size_type npos = size_type(-1);

    // construction and assignment
    constexpr basic_string_view() noexcept;
    constexpr basic_string_view(const basic_string_view&) noexcept = default;
    constexpr basic_string_view& operator=(const basic_string_view&) noexcept = default;
    constexpr basic_string_view(const CharT* str);
    constexpr basic_string_view(nullptr_t) = delete;
    constexpr basic_string_view(const CharT* str, size_type len);
    template<class It, class End>
      constexpr basic_string_view(It begin, End end);
    template<class R>
      constexpr explicit basic_string_view(R&& r);

    // iterator support
    constexpr const_iterator begin() const noexcept;
    constexpr const_iterator end() const noexcept;
    constexpr const_iterator cbegin() const noexcept;
    constexpr const_iterator cend() const noexcept;
    constexpr const_reverse_iterator rbegin() const noexcept;
    constexpr const_reverse_iterator rend() const noexcept;
    constexpr const_reverse_iterator crbegin() const noexcept;
    constexpr const_reverse_iterator crend() const noexcept;

    // capacity
    constexpr size_type size() const noexcept;
    constexpr size_type length() const noexcept;
    constexpr size_type max_size() const noexcept;
    [[nodiscard]] constexpr bool empty() const noexcept;

    // element access
    constexpr const_reference operator[](size_type pos) const;
    constexpr const_reference at(size_type pos) const;
    constexpr const_reference front() const;
    constexpr const_reference back() const;
    constexpr const_pointer data() const noexcept;

    // modifiers
    constexpr void remove_prefix(size_type n);
    constexpr void remove_suffix(size_type n);
    constexpr void swap(basic_string_view& s) noexcept;

    // string operations
    constexpr size_type copy(CharT* s, size_type n, size_type pos = 0) const;

    constexpr basic_string_view substr(size_type pos = 0, size_type n = npos) const;

    constexpr int compare(basic_string_view s) const noexcept;
    constexpr int compare(size_type pos1, size_type n1, basic_string_view s) const;
    constexpr int compare(size_type pos1, size_type n1, basic_string_view s,
                          size_type pos2, size_type n2) const;
    constexpr int compare(const CharT* s) const;
    constexpr int compare(size_type pos1, size_type n1, const CharT* s) const;
    constexpr int compare(size_type pos1, size_type n1, const CharT* s,
                          size_type n2) const;

    constexpr bool starts_with(basic_string_view x) const noexcept;
    constexpr bool starts_with(CharT x) const noexcept;
    constexpr bool starts_with(const CharT* x) const;
    constexpr bool ends_with(basic_string_view x) const noexcept;
    constexpr bool ends_with(CharT x) const noexcept;
    constexpr bool ends_with(const CharT* x) const;

    constexpr bool contains(basic_string_view x) const noexcept;
    constexpr bool contains(CharT x) const noexcept;
    constexpr bool contains(const CharT* x) const;

    // searching
    constexpr size_type find(basic_string_view s, size_type pos = 0) const noexcept;
    constexpr size_type find(CharT c, size_type pos = 0) const noexcept;
    constexpr size_type find(const CharT* s, size_type pos, size_type n) const;
    constexpr size_type find(const CharT* s, size_type pos = 0) const;
    constexpr size_type rfind(basic_string_view s, size_type pos = npos) const noexcept;
    constexpr size_type rfind(CharT c, size_type pos = npos) const noexcept;
    constexpr size_type rfind(const CharT* s, size_type pos, size_type n) const;
    constexpr size_type rfind(const CharT* s, size_type pos = npos) const;

    constexpr size_type find_first_of(basic_string_view s,
                                      size_type pos = 0) const noexcept;
    constexpr size_type find_first_of(CharT c, size_type pos = 0) const noexcept;
    constexpr size_type find_first_of(const CharT* s, size_type pos, size_type n) const;
    constexpr size_type find_first_of(const CharT* s, size_type pos = 0) const;
    constexpr size_type find_last_of(basic_string_view s,
                                     size_type pos = npos) const noexcept;
    constexpr size_type find_last_of(CharT c, size_type pos = npos) const noexcept;
    constexpr size_type find_last_of(const CharT* s, size_type pos, size_type n) const;
    constexpr size_type find_last_of(const CharT* s, size_type pos = npos) const;
    constexpr size_type find_first_not_of(basic_string_view s,
                                          size_type pos = 0) const noexcept;
    constexpr size_type find_first_not_of(CharT c, size_type pos = 0) const noexcept;
    constexpr size_type find_first_not_of(const CharT* s, size_type pos,
                                          size_type n) const;
    constexpr size_type find_first_not_of(const CharT* s, size_type pos = 0) const;
    constexpr size_type find_last_not_of(basic_string_view s,
                                       size_type pos = npos) const noexcept;
    constexpr size_type find_last_not_of(CharT c, size_type pos = npos) const noexcept;
    constexpr size_type find_last_not_of(const CharT* s, size_type pos,
                                         size_type n) const;
    constexpr size_type find_last_not_of(const CharT* s, size_type pos = npos) const;

  private:
    const_pointer data_;          // exposition only
    size_type size_;              // exposition only
  };

  // deduction guides
  template<class It, class End>
    basic_string_view(It, End) -> basic_string_view<iter_value_t<It>>;

  template<class R>
    basic_string_view(R&&) -> basic_string_view<ranges::range_value_t<R>>;
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/string_view*
