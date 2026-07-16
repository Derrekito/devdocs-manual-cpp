# std::basic_string<CharT,Traits,Allocator>::data

Returns a pointer to the underlying character array. Since C++11 this
is identical to `c_str()` — null-terminated, same pointer, same
invalidation rules. The one difference: since C++17, the non-`const`
overload returns a writable `CharT*`, so you can mutate characters in
place through `data()` without going through `operator[]`.

```cpp skip
const char* p = s.data();  // read-only, since C++11: same as c_str()
char* p = s.data();        // (since C++17) writable, same buffer
```

### What you provide

Nothing — takes no arguments. Overload resolution picks the `const`
or non-`const` version based on whether `s` itself is `const`.

### Guarantees and costs

- Constant complexity: no copy, no allocation.
- `[data(), data() + size()]` is valid (since C++11), with
  `data()[size()] == CharT()` as the terminator.
  `data() + i == std::addressof(operator[](i))` for `i` in
  `[0, size()]`.
- If `empty()` is true, the pointer is non-null and points at a single
  null character — never dereference-and-write past it.
- `noexcept`; `constexpr` since C++20. The non-`const`,
  writable overload exists (since C++17).
- Invalidated by the same events as `c_str()`: passing a non-const
  reference to the string into standard library code, or calling any
  non-const member function other than `operator[]`, `at()`,
  `front()`, `back()`, `begin()`, `end()`, `rbegin()`, `rend()`.

### Gotchas

- Through the `const` overload, writing to the array is undefined
  behavior even though C++11 guarantees it's null-terminated —
  read-only means read-only.
- Through the (since C++17) writable overload, you may overwrite any
  of the `size()` characters, but changing the terminator at
  `data() + size()` to anything other than `CharT()` is undefined
  behavior — don't smash the null.
- Before C++11, `data()` was *not* guaranteed null-terminated and
  could return a dangling pointer on an empty string; if you support
  pre-C++11 semantics elsewhere, don't assume this page's guarantees
  apply.

### Example

```cpp c++20
#include <cassert>
#include <cstring>
#include <iostream>
#include <string>

int main()
{
    std::string s("Emplary");
    assert(s.size() == std::strlen(s.data()));
    assert('\0' == *(s.data() + s.size()));

    s.data()[0] = 'e';  // (since C++17) writable
    std::cout << s << '\n';
}
```

```text
emplary
```

### Reference

```cpp skip
const CharT* data() const;  // (until C++11)
const CharT* data() const noexcept;  // (since C++11) (until C++20)
constexpr const CharT* data() const noexcept;  // (since C++20)
CharT* data() noexcept;  // (since C++17) (until C++20)
constexpr CharT* data() noexcept;  // (since C++20)
```

Before C++11, only `[data(), data() + size())` was guaranteed valid,
the array wasn't required to be null-terminated, and an empty string
could return a non-null pointer that must not be dereferenced.

### See also

- **c_str** — same pointer, same guarantees, since C++11
- **front** — accesses the first character
- **back** — accesses the last character
- **size** — returns the number of characters

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/data*
