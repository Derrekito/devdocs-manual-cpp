# sizeof... operator (since C++11)

Queries the number of elements in a parameter pack.

### Syntax

```text
sizeof...( parameter-pack )
```

Returns a constant of type `std::size_t`.

### Explanation

Returns the number of elements in a parameter pack.

### Keywords

`sizeof`

### Example

```cpp
#include <array>
#include <iostream>
#include <type_traits>

template<typename... Ts>
constexpr auto make_array(Ts&&... ts)
{
    using CT = std::common_type_t<Ts...>;
    return std::array<CT, sizeof...(Ts)>{std::forward<CT>(ts)...};
}

int main()
{
    std::array<double, 4ul> arr = make_array(1, 2.71f, 3.14, '*');
    std::cout << "arr = { ";
    for (auto s{arr.size()}; double elem : arr)
        std::cout << elem << (--s ? ", " : " ");
    std::cout << "}\n";
}
```

Output:

```text
arr = { 1, 2.71, 3.14, 42 }
```

### See also

- `sizeof`

---
*Source: https://en.cppreference.com/w/cpp/language/sizeof...*
