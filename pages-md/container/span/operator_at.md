# std::span<T,Extent>::operator[]

```cpp
constexpr reference operator[]( size_type idx ) const;  // (since C++20)
```

Returns a reference to the `idx`th element of the sequence. The behavior is
undefined if `idx` is out of range (i.e., if it is greater than or equal to
`size()`).

### Parameters

- **idx** — the index of the element to access

### Return value

A reference to the `idx`th element of the sequence, i.e., `data()[idx]`.

### Exceptions

Throws nothing.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <span>
#include <utility>

void reverse(std::span<int> span)
{
    for (std::size_t i = 0, j = std::size(span); i < j; ++i)
    {
        --j;
        std::swap(span[i], span[j]);
    }
}

void print(std::span<const int> const span)
{
    for (int element : span)
        std::cout << element << ' ';
    std::cout << '\n';
}

int main()
{
    int data[]{1, 2, 3, 4, 5};
    print(data);
    reverse(data);
    print(data);
}
```

Output:

```text
1 2 3 4 5
5 4 3 2 1
```

### See also

- **at (C++26)** — access specified element with bounds checking (public member
  function)
- **data** — direct access to the underlying contiguous storage (public member
  function)
- **size** — returns the number of elements (public member function)
- **as_bytesas_writable_bytes (C++20)** — converts a `span` into a view of its
  underlying bytes (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/span/operator_at*
