# deduction guides for `std::valarray`

```cpp
template< typename T, std::size_t cnt >
valarray( const T(&)[cnt], std::size_t ) -> valarray<T>;  // (since C++17)
```

This deduction guide is provided for `std::valarray` to allow deduction from
array and size (note that deduction from pointer and size is covered by the
implicit guides).

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    int a[] = {1, 2, 3, 4};
    std::valarray va(a, 3); // uses explicit deduction guide
    for (int x : va)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

Output:

```text
1 2 3
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/deduction_guides*
