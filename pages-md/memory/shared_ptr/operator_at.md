# std::shared_ptr<T>::operator[]

```cpp
element_type& operator[]( std::ptrdiff_t idx ) const;  // (since C++17)
```

Index into the array pointed to by the stored pointer.

The behavior is undefined if the stored pointer is null or if `idx` is negative.

If `T` (the template parameter of `shared_ptr`) is an array type `U[N]`, `idx`
must be less than `N`, otherwise the behavior is undefined.

### Parameters

- **idx** — the array index

### Return value

A reference to the `idx`-th element of the array, i.e., `get()[idx]`.

### Exceptions

Throws nothing.

### Remarks

When `T` is not an array type, it is unspecified whether this function is
declared. If the function is declared, it is unspecified what its return type
is, except that the declaration (although not necessarily the definition) of the
function is guaranteed to be legal.

### Example

```cpp
#include <cstddef>
#include <iostream>
#include <memory>

int main()
{
    const std::size_t arr_size = 10;
    std::shared_ptr<int[]> pis(new int[10]{0, 1, 2, 3, 4, 5, 6, 7, 8, 9});
    for (std::size_t i = 0; i < arr_size; ++i)
        std::cout << pis[i] << ' ';
    std::cout << '\n';
}
```

Output:

```text
0 1 2 3 4 5 6 7 8 9
```

### See also

- **get** — returns the stored pointer (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/operator_at*
