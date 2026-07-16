# std::indirect_array

```cpp
template< class T > class indirect_array;
```

`std::indirect_array` is a helper template used by the valarray subscript
operator with `std::valarray<std::size_t>` argument. It has reference semantics
to a subset of the array whose indices specified by the
`std::valarray<std::size_t>` object.

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs a `indirect_array` (public member function)
- **(destructor)** — destroys a `indirect_array` (public member function)
- **operator=** — assigns contents (public member function)
-
  **operator+=operator-=operator*=operator/=operator%=operator&=operator|=operator^=operator<<=operator>>=**
  — performs arithmetic operation on the array referred by indirect array.
  (public member function)

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> data{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};

    std::valarray<std::size_t> idx{0, 2, 4, 6, 8};

    std::cout << "Original valarray: ";
    for (int n : data)
        std::cout << n << ' ';
    std::cout << '\n';

    data[idx] += data[idx]; // double the values at indexes 'idx'

    // the type of data[idx] is std::indirect_array<int>

    std::cout << "After indirect modification: ";
    for (int n : data)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
Original valarray: 0 1 2 3 4 5 6 7 8 9
After indirect modification: 0 1 4 3 8 5 12 7 16 9
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/indirect_array*
