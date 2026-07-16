# std::ranges::view_interface<D>::data

```cpp
constexpr auto data()
    requires std::contiguous_iterator<ranges::iterator_t<D>>;  // (1) (since C++20)
constexpr auto data() const
    requires ranges::range<const D> &&
        std::contiguous_iterator<ranges::iterator_t<const D>>;  // (2) (since C++20)
```

The default implementation of `data()` member function obtains the address
denoted by the beginning iterator via `std::to_address`, which is also the
lowest address of the contiguous storage (implied by `contiguous_iterator`)
referenced by the view of the derived type when the view is not empty.

1) Let `derived` be `static_cast<D&>(*this)`. Equivalent to `return
   std::to_address(ranges::begin(derived));`.

2) Same as (1), except that `derived` is `static_cast<const D&>(*this)`.

### Parameters

(none)

### Return value

The address denoted by the beginning iterator.

### Notes

Following derived types may use the default implementation of `data()`:

- `std::ranges::common_view`
- `std::ranges::drop_view`
- `std::ranges::drop_while_view`
- `std::ranges::ref_view`
- `std::ranges::subrange`
- `std::ranges::take_view`
- `std::ranges::take_while_view`

Following types are derived from `std::ranges::view_interface` and do not
declare their own `data()` member function, but they cannot use the default
implementation, because their iterator types never satisfy
`contiguous_iterator`:

- `std::ranges::basic_istream_view`
- `std::ranges::elements_view`
- `std::ranges::filter_view`
- `std::ranges::iota_view`
- `std::ranges::join_view`
- `std::ranges::lazy_split_view`
- `std::ranges::reverse_view`
- `std::ranges::split_view`
- `std::ranges::transform_view`

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>
#include <string_view>

int main() {
    constexpr std::string_view str { "Hello, C++20!" };
    std::cout << (str | std::views::drop(7)).data() << '\n';
    constexpr static std::array a { 1,2,3,4,5 };
    constexpr auto v { a | std::views::take(3) };
    static_assert( &a[0] == v.data() );
}
```

Output:

```text
C++20!
```

### See also

- **data (C++17)** — obtains the pointer to the underlying array (function
  template)
- **ranges::data (C++20)** — obtains a pointer to the beginning of a contiguous
  range (customization point object)
- **ranges::cdata (C++20)** — obtains a pointer to the beginning of a read-only
  contiguous range (customization point object)
- **to_address (C++20)** — obtains a raw pointer from a pointer-like type
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface/data*
