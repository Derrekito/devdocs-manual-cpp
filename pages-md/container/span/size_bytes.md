# std::span<T,Extent>::size_bytes

```cpp
constexpr size_type size_bytes() const noexcept;  // (since C++20)
```

Returns the size of the sequence in bytes.

### Parameters

(none)

### Return value

The size of the sequence in bytes, i.e., `size() * sizeof(element_type)`.

### Example

```cpp
#include <cstdint>
#include <span>

int main()
{
    constexpr static std::int32_t a[]{1, 2, 3, 4, 5};
    constexpr static std::span s{a};

    static_assert
    (
        sizeof(int32_t) == 4 &&
        std::size(a) == 5 &&
        sizeof a == 20 &&
        s.size() == 5 &&
        s.size_bytes() == 20
    );
}
```

### See also

- **size (C++20)** — returns the number of elements in the sequence (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/size_bytes*
