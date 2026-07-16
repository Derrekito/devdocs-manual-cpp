# std::valarray<T>::size

```cpp
std::size_t size() const;
```

Returns the number of elements in the valarray.

### Parameters

(none)

### Return value

Number of elements in the valarray.

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    std::valarray<double> a = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::cout << "Average: " << a.sum()/a.size() << '\n';
}
```

Output:

```text
Average: 5.5
```

### See also

- **resize** — changes the size of valarray (public member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/size*
