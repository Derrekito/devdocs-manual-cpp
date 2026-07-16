# std::span<T,Extent>::subspan

```cpp
template< std::size_t Offset,
          std::size_t Count = std::dynamic_extent >
constexpr std::span<element_type, E /* see below */>
    subspan() const;  // (1) (since C++20)
constexpr std::span<element_type, std::dynamic_extent>
    subspan( size_type Offset,
             size_type Count = std::dynamic_extent ) const;  // (2) (since C++20)
```

Obtains a span that is a view over the `Count` elements of this span starting at
offset `Offset`. If `Count` is `std::dynamic_extent`, the number of elements in
the subspan is `size() - offset` (i.e., it ends at the end of `*this`).

1) Is ill-formed if

- `Offset` is greater than `Extent`, or
- `Count` is not `std::dynamic_extent` and `Count` is greater than `Extent -
  Offset`.

The behavior is undefined if either `Offset` or `Count` is out of range. This
happens if

- `Offset` is greater than `size()`, or
- `Count` is not `std::dynamic_extent` and `Count` is greater than `size() -
  Offset`.

The extent `E` of the span returned by (1) is determined as follows:

- If `Count` is not `std::dynamic_extent`, `Count`;
- Otherwise, if `Extent` is not `std::dynamic_extent`, `Extent - Offset`;
- Otherwise, `std::dynamic_extent`.

### Return value

The requested subspan `r`, such that `r.data() == this->data() + Offset`. If
`Count` is `std::dynamic_extent`, `r.size() == this->size() - Offset`; otherwise
`r.size() == Count`.

### Example

```cpp
#include <algorithm>
#include <cstdio>
#include <numeric>
#include <ranges>
#include <span>

void display(std::span<const char> abc)
{
    const auto columns{20U};
    const auto rows{abc.size() - columns + 1};

    for (auto offset{0U}; offset < rows; ++offset)
    {
        std::ranges::for_each(abc.subspan(offset, columns), std::putchar);
        std::putchar('\n');
    }
}

int main()
{
    char abc[26];
    std::iota(std::begin(abc), std::end(abc), 'A');
    display(abc);
}
```

Output:

```text
ABCDEFGHIJKLMNOPQRST
BCDEFGHIJKLMNOPQRSTU
CDEFGHIJKLMNOPQRSTUV
DEFGHIJKLMNOPQRSTUVW
EFGHIJKLMNOPQRSTUVWX
FGHIJKLMNOPQRSTUVWXY
GHIJKLMNOPQRSTUVWXYZ
```

### See also

- **first** — obtains a subspan consisting of the first `N` elements of the
  sequence (public member function)
- **last** — obtains a subspan consisting of the last `N` elements of the
  sequence (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/subspan*
