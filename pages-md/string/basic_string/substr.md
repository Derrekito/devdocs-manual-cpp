# std::basic_string<CharT,Traits,Allocator>::substr

Returns a **copy** of the characters `[pos, pos + count)` as a new
string — the original is untouched. If `count` reaches past the end
(or is `npos`, the default), the copy runs to `size()` instead of
throwing. The one way to get an exception here is `pos > size()`.

```cpp skip
s.substr();            // whole string, copied
s.substr(pos);         // from pos to the end
s.substr(pos, count);  // count chars starting at pos (clamped to size())
```

Since C++23, calling `substr()` on an rvalue string moves out of `*this`
instead of copying, so `std::move(s).substr(pos, count)` is cheaper.

### What you provide

- **pos** — index of the first character to include (default `0`).
- **count** — number of characters to include (default `npos`, meaning
  "to the end"). Values that overshoot are silently clamped, not an
  error.

### Guarantees and costs

- Always allocates and copies: linear in the resulting length. There
  is no non-owning "slice" here — for that, build a
  `std::string_view` over the same range instead.
- Throws `std::out_of_range` if `pos > size()` (note: `pos == size()`
  is fine and yields an empty string). Strong exception safety: on
  throw, `*this` is unchanged.
- The returned string's allocator is default-constructed, not a copy
  of `get_allocator()`.
- (since C++23) calling on an rvalue (`std::move(s).substr(...)`)
  moves from `*this` instead of copying.

### Gotchas

- `count` is a *length*, not an end index — `s.substr(5, 3)` is 3
  characters starting at 5, not "up to index 3".
- Passing `size() + 1` (or anything past it) as `pos` throws; passing
  it as `count` alone does not — clamping only applies to `count`.
- Every call allocates a new string. In a hot loop, prefer
  `std::string_view::substr` (no allocation) if you don't need
  ownership.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    std::string a = "0123456789abcdefghij";

    std::string sub1 = a.substr(10);        // count is npos: to the end
    std::cout << sub1 << '\n';

    std::string sub2 = a.substr(5, 3);      // fully in bounds
    std::cout << sub2 << '\n';

    std::string sub3 = a.substr(a.size() - 3, 50);  // count clamped
    std::cout << sub3 << '\n';

    try
    {
        std::string sub4 = a.substr(a.size() + 3, 50);  // pos out of range
        std::cout << sub4 << '\n';
    }
    catch (const std::out_of_range&)
    {
        std::cout << "caught\n";
    }
}
```

```text
abcdefghij
567
hij
caught
```

### Reference

```cpp skip
basic_string substr( size_type pos = 0, size_type count = npos ) const;  // (until C++20)
constexpr basic_string
    substr( size_type pos = 0, size_type count = npos ) const;  // (since C++20) (until C++23)
constexpr basic_string
    substr( size_type pos = 0, size_type count = npos ) const&;  // (since C++23)
constexpr basic_string
    substr( size_type pos = 0, size_type count = npos ) &&;  // (since C++23)
```

The `&&`-qualified overload (since C++23) is equivalent to
`basic_string(std::move(*this), pos, count)`; the others are
equivalent to `basic_string(*this, pos, count)`. `constexpr` since
C++20. Linear complexity in `count`.

### See also

- **copy** — copies characters into a caller-provided buffer
- **length** — returns the number of characters
- **find** — finds the first occurrence of a substring
- **npos** — the special "no position / to the end" constant
- **substr** — the non-owning `std::basic_string_view` equivalent

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/substr*
