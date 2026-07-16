# std::basic_string<CharT,Traits,Allocator>::empty

Checks whether the string has zero characters (`begin() == end()`).
Prefer `s.empty()` over `s.size() == 0`: same answer, but it reads as
intent rather than arithmetic, and doesn't invite the classic
mix-up with **clear** — `empty()` only *asks* whether there are
characters; it never removes any. To remove all characters, call
`clear()`.

```cpp skip
s.empty();  // true if size() == 0, false otherwise
```

### What you provide

Nothing — takes no arguments.

### Guarantees and costs

- Constant complexity.
- `noexcept`; `constexpr` and `[[nodiscard]]` since C++20 — since
  C++20, discarding the return value is a compiler warning, because
  calling `empty()` and ignoring the result is almost always a bug.

### Gotchas

- `empty()` is a query; it never modifies the string. `clear()` is
  the mutation that removes all characters. Confusing the two either
  wipes data by accident or leaves a check silently doing nothing.
- `s.empty()` and `s.size() == 0` are equivalent in result but
  `empty()` is the idiomatic spelling — some containers (like
  `std::forward_list`) don't support O(1) `size()` at all, so the
  `empty()` habit generalizes better even though `basic_string::size`
  is always O(1).

### Example

```cpp c++20
#include <iostream>
#include <string>

int main()
{
    std::string s;
    std::cout << std::boolalpha << s.empty() << '\n';

    s = "Exemplar";
    std::cout << s.empty() << '\n';

    s.clear();
    std::cout << s.empty() << '\n';
}
```

```text
true
false
true
```

### Reference

```cpp skip
bool empty() const;  // (until C++11)
bool empty() const noexcept;  // (since C++11) (until C++20)
[[nodiscard]] constexpr bool empty() const noexcept;  // (since C++20)
```

### See also

- **size** — returns the number of characters
- **clear** — removes all characters (the mutation `empty()` is not)
- **max_size** — returns the largest size the string could reach
- **capacity** — returns the current storage capacity

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/empty*
