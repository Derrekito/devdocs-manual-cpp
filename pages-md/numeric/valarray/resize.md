# std::valarray<T>::resize

```cpp
void resize( std::size_t count, T value = T() );
```

Resizes the valarray to contain `count` elements and assigns `value` to each
element.

This functions invalidates all pointers and references to elements in the array.

### Parameters

- **count** — new size of the container
- **value** — the value to initialize the new elements with

### Return value

(none)

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> v{1, 2, 3};
    v.resize(10);
    for (int n : v)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
0 0 0 0 0 0 0 0 0 0
```

### See also

- **size** — returns the size of valarray (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/resize*
