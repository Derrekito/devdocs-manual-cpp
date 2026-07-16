# std::vector<T,Allocator>::operator[]

Returns a reference to the element at `pos` with **no bounds checking**
— an out-of-range `pos` is undefined behavior, not an exception.
Constant time. Unlike `std::map::operator[]`, it never inserts anything.

```cpp skip
v[pos]   // no bounds check; UB if pos >= size()
```

### What you provide

- **pos** — the index to access. The caller is responsible for
  `pos < size()`.

### Guarantees and costs

- Constant time — no bounds check at all.
- Returns a reference (const or non-const, matching the vector's own
  constness) to the element at `pos`.
- *(Since C++20)* usable in a `constexpr` context.

### Gotchas

- `pos >= size()` is undefined behavior, not an exception and not a
  guaranteed crash — it may silently read or write past the buffer.
- Unlike `std::map::operator[]`, this never inserts; indexing past the
  end is never a way to "create" an element.
- On an empty vector, *any* index — including `0` — is out of range.

### Example

```cpp
#include <vector>
#include <iostream>

int main()
{
    std::vector<int> numbers{2, 4, 6, 8};

    std::cout << "Second element: " << numbers[1] << '\n';

    numbers[0] = 5;

    std::cout << "All numbers:";
    for (int i : numbers)
        std::cout << ' ' << i;
    std::cout << '\n';
}
```

```text
Second element: 4
All numbers: 5 4 6 8
```

### Reference

```cpp skip
reference operator[]( size_type pos );  // (1) (constexpr since C++20)
const_reference operator[]( size_type pos ) const;  // (2) (constexpr since C++20)
```

### See also

- **at** — bounds-checked access; throws `std::out_of_range`

---
*Source: https://en.cppreference.com/w/cpp/container/vector/operator_at*
