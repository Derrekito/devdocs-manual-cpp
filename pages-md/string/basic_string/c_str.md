# std::basic_string<CharT,Traits,Allocator>::c_str

```cpp
const CharT* c_str() const;  // (until C++11)
const CharT* c_str() const noexcept;  // (since C++11) (until C++20)
constexpr const CharT* c_str() const noexcept;  // (since C++20)
```

Returns a pointer to a null-terminated character array with data equivalent to
those stored in the string.

The pointer is such that the range `[``c_str()``,``c_str() + size()``]` is valid
and the values in it correspond to the values stored in the string with an
additional null character after the last position.

The pointer obtained from `c_str()` may be invalidated by:

- Passing a non-const reference to the string to any standard library function,
  or
- Calling non-const member functions on the string, excluding `operator[]`,
  `at()`, `front()`, `back()`, `begin()`, `rbegin()`, `end()` and `rend()`(since
  C++11).

Writing to the character array accessed through `c_str()` is undefined behavior.

`c_str()` and `data()` perform the same function.
*(since C++11)*

### Parameters

(none)

### Return value

Pointer to the underlying character storage.

`c_str()[i] == operator[](i)` for every `i` in `[``​0​``,``size()``)`.
*(until C++11)*

`c_str() + i == std::addressof(operator[](i))` for every `i` in
`[``​0​``,``size()``]`.
*(since C++11)*

### Complexity

Constant.

### Notes

The pointer obtained from `c_str()` may only be treated as a pointer to a
null-terminated character string if the string object does not contain other
null characters.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <cstring>
#include <string>

extern "C" void c_func(const char* c_str)
{
    printf("c_func called with '%s'\n", c_str);
}

int main()
{
    std::string const s("Emplary");
    const char* p = s.c_str();
    assert(s.size() == std::strlen(p));
    assert(std::equal(s.begin(), s.end(), p));
    assert(std::equal(p, p + s.size(), s.begin()));
    assert('\0' == *(p + s.size()));

    c_func(s.c_str());
}
```

Output:

```text
c_func called with 'Emplary'
```

### See also

- **front (DR*)** — accesses the first character (public member function)
- **back (DR*)** — accesses the last character (public member function)
- **data** — returns a pointer to the first character of a string (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/c_str*
