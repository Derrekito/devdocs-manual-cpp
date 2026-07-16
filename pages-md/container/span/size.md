# std::span<T,Extent>::size

```cpp
constexpr size_type size() const noexcept;  // (since C++20)
```

Returns the number of elements in the span.

### Parameters

(none)

### Return value

The number of elements in the span.

### Note

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ssize` | 201902L | (C++20) | `std::ssize` and unsigned
      `std::span::size`

### Example

```cpp
#include <iostream>
#include <span>

void show_sizes(std::span<const int> span)
{
    std::cout
        << span                 .size() << ' ' // 8
        << span.first(7)        .size() << ' ' // 7
        << span.first<6>()      .size() << ' ' // 6
        << span.last(5)         .size() << ' ' // 5
        << span.last<4>()       .size() << ' ' // 4
        << span.subspan(2, 3)   .size() << ' ' // 3
        << span.subspan<3, 2>() .size() << ' ' // 2
        << '\n';
}

int main()
{
    int antique_array[]{1, 2, 3, 4, 5, 6, 7, 8};
    show_sizes(antique_array);
}
```

Output:

```text
8 7 6 5 4 3 2
```

### See also

- **(constructor)** — constructs a `span` (public member function)
- **size_bytes** — returns the size of the sequence in bytes (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/size*
