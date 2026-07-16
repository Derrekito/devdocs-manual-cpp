# std::basic_string<CharT,Traits,Allocator>::c_str

Returns a pointer to a null-terminated `CharT` array holding the
string's contents — the thing to hand to C APIs expecting a
`const char*`. Since C++11, `c_str()` and `data()` are the same
function; `c_str()` is just the name that documents intent. The
pointer stays valid only until the next call that could reallocate or
otherwise change the string.

```cpp skip
const char* p = s.c_str();  // null-terminated, read-only, O(1)
```

### What you provide

Nothing — takes no arguments.

### Guarantees and costs

- Constant complexity: no copy, no allocation.
- `[c_str(), c_str() + size()]` is valid, with `c_str()[size()] == 0`
  as the terminator. `c_str()[i] == operator[](i)` for every `i` in
  `[0, size())`.
- `noexcept` since C++11; `constexpr` since C++20.
- Invalidated by passing a non-const reference to the string to any
  standard library function, or by calling any non-const member
  function on it — except `operator[]`, `at()`, `front()`, `back()`,
  `begin()`, `rbegin()`, `end()`, `rend()` (since C++11), which don't
  invalidate it.

### Gotchas

- Writing through the returned pointer is undefined behavior — it is
  read-only despite not being `const`-qualified in every call
  context; treat it as `const CharT*` always.
- The pointer is only a valid C string if the `basic_string` itself
  has no embedded null characters — a `std::string` holding `"a\0b"`
  reports `size() == 3`, but `c_str()` looks like `"a"` to any code
  that stops at the first `'\0'`.
- Any mutating call — even one that doesn't reallocate, like
  `s += ""`  — can invalidate a previously obtained `c_str()`
  pointer; re-fetch it after every mutation instead of caching it.

### Example

```cpp c++20
#include <cassert>
#include <cstring>
#include <iostream>
#include <string>

int main()
{
    std::string const s("Emplary");
    const char* p = s.c_str();

    assert(s.size() == std::strlen(p));
    assert('\0' == *(p + s.size()));

    std::cout << p << '\n';
}
```

```text
Emplary
```

### Reference

```cpp skip
const CharT* c_str() const;  // (until C++11)
const CharT* c_str() const noexcept;  // (since C++11) (until C++20)
constexpr const CharT* c_str() const noexcept;  // (since C++20)
```

`c_str() + i == std::addressof(operator[](i))` for every `i` in
`[0, size()]` (since C++11); before that, only `c_str()[i]` for
`i < size()` was specified.

### See also

- **data** — same pointer, same guarantees, since C++11
- **front** — accesses the first character
- **back** — accesses the last character
- **size** — returns the number of characters

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/c_str*
