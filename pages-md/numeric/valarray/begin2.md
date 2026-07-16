# std::begin(std::valarray)

```cpp
template< class T >
/* see below */ begin( valarray<T>& v );  // (1) (since C++11)
template< class T >
/* see below */ begin( const valarray<T>& v );  // (2) (since C++11)
```

The overload of `std::begin` for `valarray` returns an iterator of unspecified
type referring to the first element in the numeric array.

1) The return type must

- meet the requirements of mutable LegacyRandomAccessIterator,

- model `contiguous_iterator`,
*(since C++20)*

- have a member type `value_type`, which is `T`, and
- have a member type `reference`, which is `T&`.

2) The return type must

- meet the requirements of constant LegacyRandomAccessIterator,

- model `contiguous_iterator`,
*(since C++20)*

- have a member type `value_type`, which is `T`, and
- have a member type `reference`, which is `const T&`.

The iterator returned from this function is invalidated when the member function
`resize()` is called on `v` or when the lifetime of `v` ends, whichever comes
first.

### Parameters

- **v** — a numeric array

### Return value

Iterator to the first value in the numeric array.

### Exceptions

May throw implementation-defined exceptions.

### Notes

Unlike other functions that take `std::valarray` arguments, `begin()` cannot
accept the replacement types (such as the types produced by expression
templates) that may be returned from expressions involving valarrays:
`std::begin(v1 + v2)` is not portable, `std::begin(std::valarray<T>(v1 + v2))`
has to be used instead.

The intent of this function is to allow range for loops to work with valarrays,
not to provide container semantics.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <valarray>

void show(const std::valarray<int>& v)
{
    std::for_each(std::begin(v), std::end(v), [](int c)
    {
        std::cout << c << ' ';
    });
    std::cout << '\n';
};

int main()
{
    const std::valarray<int> x{47, 70, 37, 52, 90, 23, 17, 33, 22, 16, 21, 4};
    const std::valarray<int> y{25, 31, 71, 56, 21, 21, 15, 34, 21, 27, 12, 6};

    show(x);
    show(y);

    const std::valarray<int> z{x + y};

    for (char c : z)
        std::cout << c;
}
```

Output:

```text
47 70 37 52 90 23 17 33 22 16 21 4
25 31 71 56 21 21 15 34 21 27 12 6
Hello, C++!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2058 | C++11 | 1. `begin()` was required to support replacement types 2.
      it was unspecified when the returned iterators will be invalidated | 1.
      not required 2. specified

### See also

- **std::end(std::valarray) (C++11)** — specializes `std::end` (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/begin2*
