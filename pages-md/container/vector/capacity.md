# std::vector<T,Allocator>::capacity

```cpp
size_type capacity() const;  // (until C++11)
size_type capacity() const noexcept;  // (since C++11) (until C++20)
constexpr size_type capacity() const noexcept;  // (since C++20)
```

Returns the number of elements that the container has currently allocated space
for.

### Parameters

(none)

### Return value

Capacity of the currently allocated storage.

### Complexity

Constant.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <vector>

int main()
{
    int sz = 100;
    std::vector<int> v;

    auto cap = v.capacity();
    std::cout << "Initial size: " << v.size() << ", capacity: " << cap << '\n';

    std::cout << "\nDemonstrate the capacity's growth policy."
                 "\nSize:  Capacity:  Ratio:\n" << std::left;
    while (sz-- > 0)
    {
        v.push_back(sz);
        if (cap != v.capacity())
        {
            std::cout << std::setw( 7) << v.size()
                      << std::setw(11) << v.capacity()
                      << std::setw(10) << v.capacity() / static_cast<float>(cap) << '\n';
            cap = v.capacity();
        }
    }

    std::cout << "\nFinal size: " << v.size() << ", capacity: " << v.capacity() << '\n';
}
```

Possible output:

```text
Initial size: 0, capacity: 0

Demonstrate the capacity's growth policy.
Size:  Capacity:  Ratio:
1      1          inf
2      2          2
3      4          2
5      8          2
9      16         2
17     32         2
33     64         2
65     128        2

Final size: 100, capacity: 128
```

### See also

- **size** — returns the number of elements (public member function)
- **reserve** — reserves storage (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/capacity*
