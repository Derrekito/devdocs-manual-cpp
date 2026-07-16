# std::basic_string<CharT,Traits,Allocator>::operator=

```cpp
basic_string& operator=( const basic_string& str );  // (1) (constexpr since C++20)
basic_string& operator=( basic_string&& str )
    noexcept(/* see below */);  // (2) (since C++11) (constexpr since C++20)
basic_string& operator=( const CharT* s );  // (3) (constexpr since C++20)
basic_string& operator=( CharT ch );  // (4) (constexpr since C++20)
basic_string& operator=( std::initializer_list<CharT> ilist );  // (5) (since C++11) (constexpr since C++20)
template<class StringViewLike>
basic_string& operator=( const StringViewLike& t );  // (6) (since C++17) (constexpr since C++20)
basic_string& operator=( std::nullptr_t ) = delete;  // (7) (since C++23)
```

Replaces the contents of the string.

1) Replaces the contents with a copy of `str`. If `*this` and `str` are the same
   object, this function has no effect.

2) Replaces the contents with those of `str` using SequenceContainer's move
   assignment semantics.

Unlike other sequence container move assignments, references, pointers, and
   iterators to elements of `str` may be invalidated.

3) Replaces the contents with those of null-terminated character string pointed
   to by `s` as if by `assign(s, Traits::length(s))`.

4) Replaces the contents with character `ch` as if by
   `assign(std::addressof(ch), 1)`.

5) Replaces the contents with those of the initializer list `ilist` as if by
   `assign(ilist.begin(), ilist.size())`.

6) Implicitly converts `t` to a string view `sv` as if by
   `std::basic_string_view<CharT, Traits> sv = t;`, then replaces the contents
   with those of the `sv` as if by `assign(sv)`.

This overload participates in overload resolution only if
   `std::is_convertible_v<const StringViewLike&, std::basic_string_view<CharT,
   Traits>>` is `true` and `std::is_convertible_v<const StringViewLike&, const
   CharT*>` is `false`.

7) `std::basic_string` cannot be assigned from `nullptr`.

### Parameters

- **ch** — value to initialize characters of the string with
- **str** — string to be used as source to initialize the string with
- **s** — pointer to a null-terminated character string to use as source to
  initialize the string with
- **ilist** — `std::initializer_list` to initialize the string with
- **t** — object convertible to `std::basic_string_view` to initialize the
  string with

### Return value

`*this`

### Complexity

1) Linear in size of `str`.

2) Linear in the size of `*this` (formally, each `CharT` has to be destroyed).
   If allocators do not compare equal and do not propagate, then also linear in
   the size of `str` (copy must be made).

3) Linear in size of `s`.

4) Constant.

5) Linear in size of `ilist`.

6) Linear in size of `t`.

### Exceptions

2) `noexcept` specification: `noexcept(std::allocator_traits<Allocator>::
   propagate_on_container_move_assignment::value || std::allocator_traits
   <Allocator>::is_always_equal::value)`

If the operation would result in `size() > max_size()`, throws
`std::length_error`.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <string>

int main()
{
    std::string str1;
    std::string str2{"alpha"};

    // (1) operator=(const basic_string&);
    str1 = str2;
    std::cout << std::quoted(str1) << ' '   // "alpha"
              << std::quoted(str2) << '\n'; // "alpha"

    // (2) operator=(basic_string&&);
    str1 = std::move(str2);
    std::cout << std::quoted(str1) << ' '   // "alpha"
              << std::quoted(str2) << '\n'; // "" or "alpha" (unspecified)

    // (3) operator=(const CharT*);
    str1 = "beta";
    std::cout << std::quoted(str1) << '\n'; // "beta"

    // (4) operator=(CharT);
    str1 = '!';
    std::cout << std::quoted(str1) << '\n'; // "!"

    // (5) operator=(std::initializer_list<CharT>);
    str1 = {'g', 'a', 'm', 'm', 'a'};
    std::cout << std::quoted(str1) << '\n'; // "gamma"

    // (6) operator=(const T&);
    str1 = 35U; // equivalent to str1 = static_cast<char>(35U);
    std::cout << std::quoted(str1) << '\n'; // "#" (ASCII = 35)
}
```

Possible output:

```text
"alpha" "alpha"
"alpha" ""
"beta"
"!"
"gamma"
"#"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee
  LWG 2063 | C++11 | the move assignment operator did not follow
      SequenceContainer's semantic requirement | follows
  LWG 2946 | C++17 | overload (6) caused ambiguity in some cases | avoided by
      making it a template

### See also

- **(constructor)** — constructs a `basic_string` (public member function)
- **assign** — assign characters to a string (public member function)
- **operator=** — assigns a view (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/operator%3D*
