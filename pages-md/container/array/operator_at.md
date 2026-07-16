# std::array<T,N>::operator[]

```cpp
reference operator[]( size_type pos );  // (1) (since C++11) (constexpr since C++17)
const_reference operator[]( size_type pos ) const;  // (2) (since C++11) (constexpr since C++14)
```

Returns a reference to the element at specified location `pos`. No bounds
checking is performed.

### Parameters

- **pos** — position of the element to return

### Return value

Reference to the requested element.

### Complexity

Constant.

### Notes

Unlike `std::map::operator[]`, this operator never inserts a new element into
the container. Accessing a nonexistent element through this operator is
undefined behavior.

### Example

The following code uses `operator[]` to read from and write to a
`std::array<int>`:

```cpp
#include <array>
#include <iostream>

int main()
{
    std::array<int,4> numbers{2, 4, 6, 8};

    std::cout << "Second element: " << numbers[1] << '\n';

    numbers[0] = 5;

    std::cout << "All numbers:";
    for (auto i : numbers)
        std::cout << ' ' << i;
    std::cout << '\n';
}
```

Output:

```text
Second element: 4
All numbers: 5 4 6 8
```

### See also

- **at** — access specified element with bounds checking (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/operator_at*
