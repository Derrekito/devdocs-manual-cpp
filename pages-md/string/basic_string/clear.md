# std::basic_string<CharT,Traits,Allocator>::clear

Empties the string: `size()` becomes 0, as if by `erase(begin(), end())`.
Capacity is a different story — the standard does not require it to be
kept, but every existing implementation keeps it, so a `clear()`ed
string still owns its old buffer and can refill without reallocating.

```cpp skip
s.clear();  // size() -> 0; capacity() unspecified, in practice unchanged
```

### What you provide

(no parameters)

### Guarantees and costs

- Linear in the size of the string per the standard, though existing
  implementations run in constant time.
- `noexcept` since C++11; `constexpr` since C++20.
- All pointers, references, and iterators into the string are
  invalidated.
- Capacity is unspecified by the standard but kept in practice — no
  implementation frees the buffer here. To actually release memory,
  follow up with `shrink_to_fit`.

### Gotchas

- Don't rely on `clear()` to shrink memory usage; it doesn't, even
  though nothing guarantees it won't someday.
- Every iterator/pointer/reference obtained before the call is dead
  afterward, same as `erase`.

### Example

```cpp
#include <cassert>
#include <string>

int main()
{
    std::string s{"Exemplar"};
    std::string::size_type const capacity = s.capacity();

    s.clear();
    assert(s.capacity() == capacity);  // not guaranteed, but holds today
    assert(s.empty());
    assert(s.size() == 0);
}
```

```text

```

### Reference

```cpp skip
void clear();  // (until C++11)
void clear() noexcept;  // (since C++11) (until C++20)
constexpr void clear() noexcept;  // (since C++20)
```

Equivalent to `erase(begin(), end())`.

### See also

- **erase** — removes characters from the string
- **shrink_to_fit** — frees unused capacity

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/clear*
