# std::basic_string<CharT,Traits,Allocator>::erase

```cpp
basic_string& erase( size_type index = 0, size_type count = npos );  // (until C++20)
constexpr basic_string& erase( size_type index = 0, size_type count = npos );  // (since C++20)
iterator erase( iterator position );  // (until C++11)
iterator erase( const_iterator position );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator position );  // (since C++20)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator first, const_iterator last );  // (since C++20)
```

Removes specified characters from the string.

1) Removes `std::min(count, size() - index)` characters starting at `index`.

2) Removes the character at `position`.

If `position` is not a dereferenceable iterator on `*this`, the behavior is
   undefined.

3) Removes the characters in the range `[``first``,``last``)`.

If `first` or `last` is not a valid iterator on `*this`, or
   `[``first``,``last``)` is not a valid range, the behavior is undefined.

### Parameters

- **index** — first character to remove
- **count** — number of characters to remove
- **position** — iterator to the character to remove
- **first, last** — range of the characters to remove

### Return value

1) `*this`

2) Iterator pointing to the character immediately following the character
   erased, or `end()` if no such character exists.

3) Iterator pointing to the character `last` pointed to before the erase, or
   `end()` if no such character exists.

### Exceptions

1) `std::out_of_range` if `index > size()`.

2,3) Throws nothing.

If an exception is thrown for any reason, this function has no effect (strong
exception safety guarantee).

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string s = "This Is An Example";
    std::cout << "1) " << s << '\n';

    s.erase(7, 3); // erases " An" using overload (1)
    std::cout << "2) " << s << '\n';

    s.erase(std::find(s.begin(), s.end(), ' ')); // erases first ' '; overload (2)
    std::cout << "3) " << s << '\n';

    s.erase(s.find(' ')); // trims from ' ' to the end of the string; overload (1)
    std::cout << "4) " << s << '\n';

    auto it = std::next(s.begin(), s.find('s')); // obtains iterator to the first 's'
    s.erase(it, std::next(it, 2)); // erases "sI"; overload (3)
    std::cout << "5) " << s << '\n';
}
```

Output:

```text
1) This Is An Example
2) This Is Example
3) ThisIs Example
4) ThisIs
5) This
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 27 | C++98 | overload (3) did not erase the character `last` pointed to,
      but it returned the iterator pointing to the character immediately
      following that character | returns an iterator pointing to that character
  LWG 428 | C++98 | overload (2) explicitly required `position` to be valid, but
      SequenceContainer requires it to be dereferenceable (stricter) | removed
      the explicit requirement
  LWG 847 | C++98 | there was no exception safety guarantee | added strong
      exception safety guarantee

### See also

- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/erase*
