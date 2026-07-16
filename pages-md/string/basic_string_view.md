# std::basic_string_view

```cpp
template<
    class CharT,
    class Traits = std::char_traits<CharT>
> class basic_string_view;  // (since C++17)
```

The class template `basic_string_view` describes an object that can refer to a
constant contiguous sequence of `CharT` with the first element of the sequence
at position zero.

Every specialization of `std::basic_string_view` is a TriviallyCopyable type.
*(since C++23)*

A typical implementation holds only two members: a pointer to constant `CharT`
and a size.

Several typedefs for common character types are provided:

- **`std::string_view` (C++17)** — std::basic_string_view<char>
- **`std::wstring_view` (C++17)** — std::basic_string_view<wchar_t>
- **`std::u8string_view` (C++20)** — std::basic_string_view<char8_t>
- **`std::u16string_view` (C++17)** — std::basic_string_view<char16_t>
- **`std::u32string_view` (C++17)** — std::basic_string_view<char32_t>

### Template parameters

- **CharT** — character type
- **Traits** — CharTraits class specifying the operations on the character type.
  Like for `std::basic_string`, `Traits::char_type` must name the same type as
  `CharT` or the program is ill-formed.

### Member types

- **`traits_type`** — `Traits`
- **`value_type`** — `CharT`
- **`pointer`** — CharT*
- **`const_pointer`** — const CharT*
- **`reference`** — CharT&
- **`const_reference`** — const CharT&
- **`const_iterator`** — implementation-defined constant
  LegacyRandomAccessIterator, and LegacyContiguousIterator (until C++20)
  ConstexprIterator, and `contiguous_iterator` (since C++20) whose `value_type`
  is `CharT`
- **and LegacyContiguousIterator** — (until C++20)
- **ConstexprIterator, and `contiguous_iterator`** — (since C++20)
- **`iterator`** — `const_iterator`
- **`const_reverse_iterator`** — std::reverse_iterator<const_iterator>
- **`reverse_iterator`** — `const_reverse_iterator`
- **`size_type`** — `std::size_t`
- **`difference_type`** — `std::ptrdiff_t`

Note: `iterator` and `const_iterator` are the same type because string views are
views into constant character sequences.

All requirements on the iterator types of a Container applies to the `iterator`
and `const_iterator` types of `basic_string_view` as well.

### Member functions

**Constructors and assignment**

- **(constructor)** — constructs a `basic_string_view` (public member function)
- **operator=** — assigns a view (public member function)

**Iterators**

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend** — returns an iterator to the end (public member function)
- **rbegincrbegin** — returns a reverse iterator to the beginning (public member
  function)
- **rendcrend** — returns a reverse iterator to the end (public member function)

**Element access**

- **operator[]** — accesses the specified character (public member function)
- **at** — accesses the specified character with bounds checking (public member
  function)
- **front** — accesses the first character (public member function)
- **back** — accesses the last character (public member function)
- **data** — returns a pointer to the first character of a view (public member
  function)

**Capacity**

- **sizelength** — returns the number of characters (public member function)
- **max_size** — returns the maximum number of characters (public member
  function)
- **empty** — checks whether the view is empty (public member function)

**Modifiers**

- **remove_prefix** — shrinks the view by moving its start forward (public
  member function)
- **remove_suffix** — shrinks the view by moving its end backward (public member
  function)
- **swap** — swaps the contents (public member function)

**Operations**

- **copy** — copies characters (public member function)
- **substr** — returns a substring (public member function)
- **compare** — compares two views (public member function)
- **starts_with (C++20)** — checks if the string view starts with the given
  prefix (public member function)
- **ends_with (C++20)** — checks if the string view ends with the given suffix
  (public member function)
- **contains (C++23)** — checks if the string view contains the given substring
  or character (public member function)
- **find** — find characters in the view (public member function)
- **rfind** — find the last occurrence of a substring (public member function)
- **find_first_of** — find first occurrence of characters (public member
  function)
- **find_last_of** — find last occurrence of characters (public member function)
- **find_first_not_of** — find first absence of characters (public member
  function)
- **find_last_not_of** — find last absence of characters (public member
  function)

**Constants**

- **npos [static]** — special value. The exact meaning depends on the context
  (public static member constant)

### Non-member functions

- **operator==operator!=operator<operator>operator<=operator>=operator<=>
  (C++17)(removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares two string
  views (function template)

**Input/output**

- **operator<< (C++17)** — performs stream output on string views (function
  template)

### Literals

- **operator""sv (C++17)** — creates a string view of a character array literal
  (function)

### Helper classes

-
  **std::hash<std::string_view>std::hash<std::wstring_view>std::hash<std::u8string_view>std::hash<std::u16string_view>std::hash<std::u32string_view>
  (C++17)(C++17)(C++20)(C++17)(C++17)** — hash support for string views (class
  template specialization)

### Helper templates

```cpp
template< class CharT, class Traits >
inline constexpr bool
    ranges::enable_borrowed_range<std::basic_string_view<CharT, Traits>> = true;  // (since C++20)
```

This specialization of `ranges::enable_borrowed_range` makes `basic_string_view`
satisfy `borrowed_range`.

```cpp
template< class CharT, class Traits >
inline constexpr bool
    ranges::enable_view<std::basic_string_view<CharT, Traits>> = true;  // (since C++20)
```

This specialization of `ranges::enable_view` makes `basic_string_view` satisfy
`view`.

### Deduction guides
*(since C++20)*

### Notes

It is the programmer's responsibility to ensure that `std::string_view` does not
outlive the pointed-to character array:

```cpp
std::string_view good{"a string literal"};
    // "Good" case: `good` points to a static array.
    // String literals reside in persistent data storage.

std::string_view bad{"a temporary string"s};
    // "Bad" case: `bad` holds a dangling pointer since the std::string temporary,
    // created by std::operator""s, will be destroyed at the end of the statement.
```

Specializations of `std::basic_string_view` are already trivially copyable types
in all existing implementations, even before the formal requirement introduced
in C++23.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_string_view` | 201606L | (C++17) | `std::string_view`
  201803L | (C++20) | ConstexprIterator
  `__cpp_lib_string_contains` | 202011L | (C++23) | `contains`

### Example

```cpp
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view unicode[] { "▀▄─", "▄▀─", "▀─▄", "▄─▀" };

    for (int y{}, p{}; y != 6; ++y, p = ((p + 1) % 4))
    {
        for (int x{}; x != 16; ++x)
            std::cout << unicode[p];
        std::cout << '\n';
    }
}
```

Output:

```text
▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─
▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─
▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄
▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀
▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─▀▄─
▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─▄▀─
```

### See also

- **basic_string** — stores and manipulates sequences of characters (class
  template)
- **span (C++20)** — a non-owning view over a contiguous sequence of objects
  (class template)
- **initializer_list (C++11)** — creates a temporary array in
  list-initialization and then references it (class template)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string_view*
