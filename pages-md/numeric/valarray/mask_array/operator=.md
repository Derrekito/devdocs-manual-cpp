# std::mask_array<T>::operator=

```cpp
void operator=( const T& value ) const;  // (1)
void operator=( const std::valarray<T>& val_arr ) const;  // (2)
const mask_array& operator=( const mask_array& other_arr ) const;  // (3)
```

Assigns values to all referred elements.

1) Assigns `value` to all of the elements.

2) Assigns the elements of `val_arr` to the referred to elements of `*this`.

3) Assigns the selected elements from `other_arr` to the referred to elements of
   `*this`.

### Parameters

- **value** — a value to assign to all of the referred elements
- **val_arr** — `std::valarray` to assign
- **other_arr** — `std::mask_array` to assign

### Return value

1,2) (none)

3) `*this`

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <valarray>

void print(std::valarray<int> const& v)
{
    for (int e : v)
        std::cout << std::setw(2) << e << ' ';
    std::cout << '\n';
}

int main()
{
    const auto init = {1, 2, 3, 4, 5, 6, 7, 8};
    std::valarray<int> v;

    v = init;
    v[(v % 2) == 0] = 0; // (1)
    print(v);

    v = init;
    v[(v % 2) == 1] = std::valarray<int>{-1, -2, -3, -4}; // (2)
    print(v);

    v = init;
    v[(v % 2) == 0] = v[(v % 2) == 1]; // (3)
    print(v);
}
```

Output:

```text
 1  0  3  0  5  0  7  0
-1  2 -2  4 -3  6 -4  8
 1  1  3  3  5  5  7  7
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 123 | C++98 | overload (2) was non-const | made const
  LWG 253 | C++98 | the copy assignment operator was private | made public
  LWG 621 | C++98 | the copy assignment operator was non-const | made const

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/mask_array/operator%3D*
