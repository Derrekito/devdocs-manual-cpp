# std::basic_string<CharT,Traits,Allocator>::data

```cpp
const CharT* data() const;  // (until C++11)
const CharT* data() const noexcept;  // (since C++11) (until C++20)
constexpr const CharT* data() const noexcept;  // (since C++20)
CharT* data() noexcept;  // (since C++17) (until C++20)
constexpr CharT* data() noexcept;  // (since C++20)
```

Returns a pointer to the underlying array serving as character storage. The
pointer is such that the range

`[``data()``,``data() + size()``)`
*(until C++11)*

`[``data()``,``data() + size()``]`
*(since C++11)*

is valid and the values in it correspond to the values stored in the string.

The returned array is not required to be null-terminated.
If `empty()` returns `true`, the pointer is a non-null pointer that should not
be dereferenced.
*(until C++11)*

The returned array is null-terminated, that is, `data()` and `c_str()` perform
the same function.
If `empty()` returns `true`, the pointer points to a single null character.
*(since C++11)*

The pointer obtained from `data()` may be invalidated by:

- Passing a non-const reference to the string to any standard library function,
  or
- Calling non-const member functions on the string, excluding `operator[]()`,
  `at()`, `front()`, `back()`, `begin()`, `end()`, `rbegin()`, `rend()`.

1) Modifying the character array accessed through the const overload of `data`
   has undefined behavior.

2) Modifying the past-the-end null terminator stored at `data() +``size()` to
   any value other than `CharT()` has undefined behavior.

### Parameters

(none)

### Return value

A pointer to the underlying character storage.

`data()[i] == operator[](i)` for every `i` in `[``​0​``,``size()``)`.
*(until C++11)*

`data() + i == std::addressof(operator[](i))` for every `i` in
`[``​0​``,``size()``]`.
*(since C++11)*

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <cstring>
#include <string>

int main()
{
    std::string const s("Emplary");
    assert(s.size() == std::strlen(s.data()));
    assert(std::equal(s.begin(), s.end(), s.data()));
    assert(std::equal(s.data(), s.data() + s.size(), s.begin()));
    assert('\0' == *(s.data() + s.size()));
}
```

### See also

- **front (DR*)** — accesses the first character (public member function)
- **back (DR*)** — accesses the last character (public member function)
- **c_str** — returns a non-modifiable standard C character array version of the
  string (public member function)
- **data** — returns a pointer to the first character of a view (public member
  function of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/data*
