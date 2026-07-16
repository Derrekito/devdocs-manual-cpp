# std::span<T,Extent>::front

```cpp
constexpr reference front() const;  // (since C++20)
```

Returns a reference to the first element in the span.

Calling `front` on an empty span results in undefined behavior.

### Parameters

(none)

### Return value

A reference to the first element.

### Complexity

Constant.

### Notes

For a span `c`, the expression `c.front()` is equivalent to `*c.begin()`.

### Example

```cpp
#include <iostream>
#include <span>

void print(std::span<const int> const data)
{
    for (auto offset{0U}; offset != data.size(); ++offset)
        std::cout << data.subspan(offset).front() << ' ';
    std::cout << '\n';
}

int main()
{
    constexpr int data[]{0, 1, 2, 3, 4, 5, 6};
    print({data, 4});
}
```

Output:

```text
0 1 2 3
```

### See also

- **back** — access the last element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/front*
