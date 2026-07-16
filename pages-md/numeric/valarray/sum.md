# std::valarray<T>::sum

```cpp
T sum() const;
```

Computes the sum of the elements.

The function can be used only if `operator+=` is defined for type `T`. If the
`std::valarray` is empty, the behavior is undefined. The order in which the
elements are processed by this function is unspecified.

### Parameters

(none)

### Return value

The sum of the elements.

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<int> a = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::cout << a.sum() << '\n';
}
```

Output:

```text
55
```

### See also

- **apply** — applies a function to every element of a valarray (public member
  function)
- **accumulate** — sums up or folds a range of elements (function template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/sum*
