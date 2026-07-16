# std::counted_iterator<I>::count

```cpp
constexpr std::iter_difference_t<I> count() const noexcept;  // (since C++20)
```

Returns the underlying *length* that is the distance to the end.

### Parameters

(none)

### Return value

The underlying *length*.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <iterator>

int main()
{
    constexpr static auto il = {1, 2, 3, 4, 5};
    constexpr std::counted_iterator i1{il.begin() + 1, 3};
    static_assert(i1.count() == 3);
    auto i2{i1};
    for (; std::default_sentinel != i2; ++i2)
        std::cout << "*i2: " << *i2 << ", count(): " << i2.count() << '\n';
    assert(i2.count() == 0);
}
```

Output:

```text
*i2: 2, count(): 3
*i2: 3, count(): 2
*i2: 4, count(): 1
```

### See also

- **base (C++20)** — accesses the underlying iterator (public member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/count*
