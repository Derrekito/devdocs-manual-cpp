# std::valarray<T>::max

```cpp
T max() const;
```

Computes the maximum value of the elements.

If there are no elements, the behavior is undefined.

The function can be used only if `operator<` is defined for type `T`.

### Parameters

(none)

### Return value

The maximum of the elements.

### Example

```cpp
#include <valarray>
#include <iostream>

int main()
{
    std::valarray<double> a{1, 2, 3, 4, 5, 6, 7, 8};
    std::cout << "Maximum value : " << a.max() << "\n";
}
```

Output:

```text
Maximum value : 8
```

### See also

- **min** — returns the smallest element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/max*
