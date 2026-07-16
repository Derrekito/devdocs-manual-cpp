# std::operator+(std::basic_string)

```cpp
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const std::basic_string<CharT,Traits,Alloc>& lhs,
                   const std::basic_string<CharT,Traits,Alloc>& rhs );  // (1) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const std::basic_string<CharT,Traits,Alloc>& lhs,
                   const CharT* rhs );  // (2) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const std::basic_string<CharT,Traits,Alloc>& lhs,
                   CharT rhs );  // (3) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const CharT* lhs,
                   const std::basic_string<CharT,Traits,Alloc>& rhs );  // (4) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( CharT lhs,
                   const std::basic_string<CharT,Traits,Alloc>& rhs );  // (5) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( std::basic_string<CharT,Traits,Alloc>&& lhs,
                   std::basic_string<CharT,Traits,Alloc>&& rhs );  // (6) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( std::basic_string<CharT,Traits,Alloc>&& lhs,
                   const std::basic_string<CharT,Traits,Alloc>& rhs );  // (7) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( std::basic_string<CharT,Traits,Alloc>&& lhs,
                   const CharT* rhs );  // (8) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( std::basic_string<CharT,Traits,Alloc>&& lhs,
                   CharT rhs );  // (9) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const std::basic_string<CharT,Traits,Alloc>& lhs,
                   std::basic_string<CharT,Traits,Alloc>&& rhs );  // (10) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( const CharT* lhs,
                   std::basic_string<CharT,Traits,Alloc>&& rhs );  // (11) (since C++11) (constexpr since C++20)
template< class CharT, class Traits, class Alloc >
    std::basic_string<CharT,Traits,Alloc>
        operator+( CharT lhs,
                   std::basic_string<CharT,Traits,Alloc>&& rhs );  // (12) (since C++11) (constexpr since C++20)
```

Returns a string containing characters from `lhs` followed by the characters
from `rhs`.

The allocator used for the result is:
1-3)
`std::allocator_traits<Alloc>::select_on_container_copy_construction(lhs.get_allocator())`
4,5)
`std::allocator_traits<Alloc>::select_on_container_copy_construction(rhs.get_allocator())`
6-9) `lhs.get_allocator()`
10-12) `rhs.get_allocator()`
In other words, if one operand is a `basic_string` rvalue, its allocator is
used; otherwise, `select_on_container_copy_construction` is used on the
allocator of the lvalue `basic_string` operand. In each case, the left operand
is preferred when both are `basic_string`s of the same value category.
For (6-12), all rvalue `basic_string` operands are left in valid but unspecified
states.
*(since C++11)*

### Parameters

- **lhs** — string, character, or pointer to the first character in a
  null-terminated array
- **rhs** — string, character, or pointer to the first character in a
  null-terminated array

### Return value

A string containing characters from `lhs` followed by the characters from `rhs`,
using the allocator determined as above(since C++11).

### Notes
`operator+` should be used with great caution when stateful allocators are
involved (such as when `std::pmr::string` is used)(since C++17). Prior to
P1165R1, the allocator used for the result was determined by historical accident
and can vary from overload to overload for no apparent reason. Moreover, for
(1-5), the allocator propagation behavior varies across major standard library
implementations and differs from the behavior depicted in the standard.
Because the allocator used by the result of `operator+` is sensitive to value
category, `operator+` is not associative with respect to allocator propagation:
```cpp
using my_string = std::basic_string<char, std::char_traits<char>, my_allocator<char>>;
my_string cat();
const my_string& dog();
my_string meow = /* ... */, woof = /* ... */;
meow + cat() + /* ... */; // uses select_on_container_copy_construction on meow's allocator
woof + dog() + /* ... */; // uses allocator of dog()'s return value instead
meow + woof + meow; // uses select_on_container_copy_construction on meow's allocator
meow + (woof + meow); // uses SOCCC on woof's allocator instead
```
For a chain of `operator+` invocations, the allocator used for the ultimate
result may be controlled by prepending an rvalue `basic_string` with the desired
allocator:
```cpp
// use my_favorite_allocator for the final result
my_string(my_favorite_allocator) + meow + woof + cat() + dog();
```
For better and portable control over allocators, member functions like `append`,
`insert`, and `operator+=` should be used on a result string constructed with
the desired allocator.
*(since C++11)*

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string s1 = "Hello";
    std::string s2 = "world";
    std::cout << s1 + ' ' + s2 + "!\n";
}
```

Output:

```text
Hello world!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P1165R1 | C++11 | allocator propagation is haphazard and inconsistent | made
      more consistent

### See also

- **operator+=** — appends characters to the end (public member function)
- **append** — appends characters to the end (public member function)
- **insert** — inserts characters (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/operator%2B*
