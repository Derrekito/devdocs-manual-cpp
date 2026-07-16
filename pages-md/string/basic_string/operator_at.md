# std::basic_string<CharT,Traits,Allocator>::operator[]

```cpp
reference operator[]( size_type pos );  // (until C++20)
constexpr reference operator[]( size_type pos );  // (since C++20)
const_reference operator[]( size_type pos ) const;  // (until C++20)
constexpr const_reference operator[]( size_type pos ) const;  // (since C++20)
```

Returns a reference to the character at specified location `pos` if `pos <
size()`, or a reference to `CharT()` if `pos == size()`. No bounds checking is
performed.

If `pos > size()`, the behavior is undefined.

For overload (1), if `pos == size()`, the behavior is undefined if the object
referred by the returned reference is modified to any value other than
`CharT()`(since C++11).

### Parameters

- **pos** — position of the character to return

### Return value

`*(begin() + pos)` if `pos < size()`, or a reference to `CharT()` if `pos ==
size()`.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    const std::string e("Exemplar");
    for (unsigned i = e.length() - 1; i != 0; i /= 2)
        std::cout << e[i];
    std::cout << '\n';

    const char* c = &e[0];
    std::cout << c << '\n'; // print as a C string

    // Change the last character of s into a 'y'
    std::string s("Exemplar ");
    s[s.size() - 1] = 'y'; // equivalent to s.back() = 'y';
    std::cout << s << '\n';
}
```

Output:

```text
rmx
Exemplar
Exemplary
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 259 | C++98 | `data()[pos]` was returned if `pos < size()` (its type is
      `const CharT&`, which conflicts with `reference`) | changed to `*(begin()
      + pos)`
  LWG 2475 | C++11 | if `pos == size()`, the behavior of modifying the object
      referred by the returned reference was undefined | well-defined if
      modified to `CharT()`

### See also

- **at** — accesses the specified character with bounds checking (public member
  function)
- **front (DR*)** — accesses the first character (public member function)
- **back (DR*)** — accesses the last character (public member function)
- **operator[]** — accesses the specified character (public member function of
  `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/operator_at*
