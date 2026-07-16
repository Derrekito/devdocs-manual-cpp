# std::end(std::valarray)

```cpp
template< class T >
/* see below */ end( valarray<T>& v );  // (1) (since C++11)
template< class T >
/* see below */ end( const valarray<T>& v );  // (2) (since C++11)
```

The overload of `std::end` for `valarray` returns an iterator of unspecified
type referring to the one past the last element in the numeric array.

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

Iterator to one past the last value in the numeric array.

### Exceptions

May throw implementation-defined exceptions.

### Notes

Unlike other functions that take `std::valarray` arguments, `end()` cannot
accept the replacement types (such as the types produced by expression
templates) that may be returned from expressions involving valarrays:
`std::end(v1 + v2)` is not portable, `std::end(std::valarray<T>(v1 + v2))` has
to be used instead.

The intent of this function is to allow range for loops to work with valarrays,
not to provide container semantics.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <valarray>

int main()
{
    const std::valarray<char> va
    {
        'H', 'e', 'l', 'l', 'o',
        ',', ' ',
        'C', '+', '+', '!', '\n'
    };

    std::for_each(std::begin(va), std::end(va),
                  [](char c){ std::cout << c; });
}
```

Output:

```text
Hello, C++!
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2058 | C++11 | 1. `end()` was required to support replacement types 2. it
      was unspecified when the returned iterators will be invalidated | 1. not
      required 2. specified

### See also

- **std::begin(std::valarray) (C++11)** — overloads `std::begin` (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/end2*
