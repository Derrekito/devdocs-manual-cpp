# std::integral

```cpp
template< class T >
concept integral = std::is_integral_v<T>;  // (since C++20)
```

The concept `integral<T>` is satisfied if and only if `T` is an integral type.

### Example

```cpp
#include <concepts>
#include <iostream>

void print(std::integral auto i)
{
    std::cout << "Integral: " << i << '\n';
}

void print(auto x)
{
    std::cout << "Non-integral: " << x << '\n';
}

int main()
{
    std::cout << std::boolalpha;

    static_assert(std::integral<bool>);
    print(true);

    static_assert(std::integral<char>);
    print('o');

    static_assert(std::integral<int>);
    print(007);

    static_assert(!std::integral<double>);
    print(2e2);

    static_assert(!std::integral<decltype("")>);
    print("∫∫∫");
}
```

Output:

```text
Integral: true
Integral: o
Integral: 7
Non-integral: 200
Non-integral: ∫∫∫
```

### See also

- **is_integral (C++11)** — checks if a type is an integral type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/concepts/integral*
