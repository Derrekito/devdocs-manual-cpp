# std::swap(std::valarray)

```cpp
template< class T >
void swap( std::valarray<T>& lhs, std::valarray<T>& rhs ) noexcept;  // (since C++11)
```

Specializes the `std::swap` algorithm for `std::valarray`. Swaps the contents of
`lhs` and `rhs`. Calls `lhs.swap(rhs)`.

### Parameters

- **lhs, rhs** — valarrays whose contents to swap

### Return value

(none)

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <valarray>

void print(auto rem, const std::valarray<int>& v)
{
    std::cout << rem << '{';
    for (char sep[]{0, ' ', 0}; auto elem : v)
        std::cout << sep << elem, *sep = ',';
    std::cout << "}\n";
}

int main()
{
    std::valarray x{3, 1, 4, 1, 5};
    std::valarray y{2, 7, 1, 8};

    print("Before swap:\n" "x: ", x);
    print("y: ", y);

    std::swap(x, y);

    print("After swap:\n" "x: ", x);
    print("y: ", y);
}
```

Output:

```text
Before swap:
x: {3, 1, 4, 1, 5}
y: {2, 7, 1, 8}
After swap:
x: {2, 7, 1, 8}
y: {3, 1, 4, 1, 5}
```

### See also

- **swap** — swaps with another valarray (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/swap2*
