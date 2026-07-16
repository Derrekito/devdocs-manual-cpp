# std::mask_array

```cpp
template< class T > class mask_array;
```

`std::mask_array` is a helper template used by the valarray subscript operator
with `std::valarray<bool>` argument. It has reference semantics and provides
access to the subset of the valarray consisting of the elements whose indices
correspond to `true` values in the `std::valarray<bool>` mask.

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs a `mask_array` (public member function)
- **(destructor)** — destroys a `mask_array` (public member function)
- **operator=** — assigns contents (public member function)
-
  **operator+=operator-=operator*=operator/=operator%=operator&=operator|=operator^=operator<<=operator>>=**
  — performs arithmetic operation on the array referred by mask. (public member
  function)

### Example

```cpp
#include <iostream>
#include <valarray>

void println(auto rem, const auto& data)
{
    for (std::cout << rem; int n : data)
        std::cout << n << ' ';
    std::cout << '\n';
}

int main()
{
    std::valarray<int> data{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    println("Initial valarray: ", data);

    data[data > 5] = -1;
    // the type of data>5 is std::valarray<bool>
    // the type of data[data>5] is std::mask_array<int>

    println("After v[v>5]=-1:  ", data);
}
```

Output:

```text
Initial valarray: 0 1 2 3 4 5 6 7 8 9
After v[v>5]=-1:  0 1 2 3 4 5 -1 -1 -1 -1
```

### See also

- **simd_mask (parallelism TS v2)** — data-parallel type with the element type
  bool (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/mask_array*
