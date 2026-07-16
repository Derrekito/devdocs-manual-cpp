# std::span<T,Extent>::back

```cpp
constexpr reference back() const;  // (since C++20)
```

Returns a reference to the last element in the span.

Calling `back` on an empty span results in undefined behavior.

### Parameters

(none)

### Return value

A reference to the back element.

### Complexity

Constant.

### Notes

For a span `c`, the expression `c.back()` is equivalent to `*(c.end() - 1)`.

### Example

```cpp
#include <iostream>
#include <span>

void print_forward(std::span<const int> const span)
{
    for (auto n{span.size()}; n != 0; --n)
        std::cout << span.last(n).front() << ' ';
    std::cout << '\n';
}

void print_backward(std::span<const int> const span)
{
    for (auto n{span.size()}; n != 0; --n)
        std::cout << span.first(n).back() << ' ';
    std::cout << '\n';
}

int main()
{
    constexpr int numbers[]{0, 1, 2, 3, 4};
    print_forward(numbers);
    print_backward(numbers);
}
```

Output:

```text
0 1 2 3 4
4 3 2 1 0
```

### See also

- **front** — access the first element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/span/back*
