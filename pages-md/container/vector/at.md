# std::vector<T,Allocator>::at

Returns a reference to the element at `pos`, checking bounds first:
throws `std::out_of_range` if `pos` is out of range, instead of the
undefined behavior `operator[]` invokes. Constant time either way — reach
for it whenever `pos` might be untrusted.

```cpp skip
v.at(pos)   // throws std::out_of_range if pos >= size()
```

### What you provide

- **pos** — the index to access, checked against `size()` before use.

### Guarantees and costs

- Constant time, including the bounds check.
- Throws `std::out_of_range` if `pos >= size()`; otherwise returns a
  reference to that element (const or non-const, matching the vector's
  own constness).

### Gotchas

- The check costs real time on every call — in a proven-safe hot loop
  where indices are already validated, `operator[]` is faster; use
  `at()` when the index comes from input, a computation, or anywhere
  else it isn't already known to be in range.
- The thrown `what()` message is implementation-defined — don't parse
  it; catch `std::out_of_range` and act on that instead.

### Example

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

int main()
{
    std::vector<int> data = {1, 2, 4, 5, 5, 6};

    data.at(1) = 88;
    std::cout << "element 2: " << data.at(2) << '\n';

    try
    {
        data.at(6) = 666;
    }
    catch (std::out_of_range const&)
    {
        std::cout << "at(6) threw std::out_of_range\n";
    }

    std::cout << "data:";
    for (int elem : data)
        std::cout << ' ' << elem;
    std::cout << '\n';
}
```

```text
element 2: 4
at(6) threw std::out_of_range
data: 1 88 4 5 5 6
```

### Reference

```cpp skip
reference at( size_type pos );  // (1) (constexpr since C++20)
const_reference at( size_type pos ) const;  // (2) (constexpr since C++20)
```

### See also

- **operator[]** — same access, no bounds check (undefined behavior if
  out of range)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/at*
