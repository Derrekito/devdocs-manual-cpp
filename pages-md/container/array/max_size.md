# std::array<T,N>::max_size

```cpp
constexpr size_type max_size() const noexcept;  // (since C++11)
```

Returns the maximum number of elements the container is able to hold due to
system or library implementation limitations, i.e. `std::distance(begin(),
end())` for the largest container.

### Parameters

(none)

### Return value

Maximum number of elements.

### Complexity

Constant.

### Notes

Because each `std::array<T, N>` is a fixed-size container, the value returned by
`max_size` equals `N` (which is also the value returned by `size`).

### Example

```cpp
#include <iostream>
#include <locale>
#include <array>

int main()
{
    std::array<char, 10> q;
    std::cout.imbue(std::locale("en_US.UTF-8"));
    std::cout << "Maximum size of the std::array is " << q.max_size() << '\n';
}
```

Output:

```text
Maximum size of the std::array is 10
```

### See also

- **size** — returns the number of elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/array/max_size*
