# std::valarray<T>::min

```cpp
T min() const;
```

Computes the minimum value of the elements.

If there are no elements, the behavior is undefined.

The function can be used only if `operator<` is defined for type `T`.

### Parameters

(none)

### Return value

The minimum of the elements.

### Example

```cpp
#include <valarray>
#include <iostream>

int main()
{
    std::valarray<double> a{1, 2, 3, 4, 5, 6, 7, 8};
    std::cout << "Minimum value : " << a.min() << "\n";
}
```

Output:

```text
Minimum value : 1
```

### See also

- **max** — returns the largest element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/min*
