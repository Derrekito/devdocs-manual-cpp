# operator==,!=,<,<=,>,>=(std::valarray)

```cpp
template< class T >
std::valarray<bool> operator==( const std::valarray<T>& lhs, const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator!=( const std::valarray<T>& lhs, const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator< ( const std::valarray<T>& lhs, const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator<=( const std::valarray<T>& lhs, const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator> ( const std::valarray<T>& lhs, const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator>=( const std::valarray<T>& lhs, const std::valarray<T>& rhs );  // (1)
template< class T >
std::valarray<bool> operator==( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator!=( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator< ( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator<=( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator> ( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );
template< class T >
std::valarray<bool> operator>=( const typename std::valarray<T>::value_type & lhsv,
                                const std::valarray<T>& rhs );  // (2)
template< class T >
std::valarray<bool> operator==( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );
template< class T >
std::valarray<bool> operator!=( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );
template< class T >
std::valarray<bool> operator< ( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );
template< class T >
std::valarray<bool> operator<=( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );
template< class T >
std::valarray<bool> operator> ( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );
template< class T >
std::valarray<bool> operator>=( const std::valarray<T>& lhs,
                                const typename std::valarray<T>::value_type & rhsv );  // (3)
```

Compares each value within the numeric array with another value.

1) Returns a numeric array of `bool` containing elements each of which is
   obtained by applying the indicated comparison operator to the corresponding
   values of `lhs` and `rhs`.

The behavior is undefined if `size() != v.size()`.

2) Returns a numeric array of `bool` containing elements each of which is
   obtained by applying the indicated comparison operator to `lhsv` and the
   corresponding value of `rhs`.

3) Returns a numeric array of `bool` containing elements each of which is
   obtained by applying the indicated comparison operator to the corresponding
   value of `lhs` and `rhsv`.

### Parameters

- **lhs, rhs** — numeric arrays to compare
- **lhsv, rhsv** — values to compare to each element within a numeric array

### Return value

A numeric array of `bool` containing comparison results of corresponding
elements.

### Exceptions

May throw implementation-defined exceptions.

### Notes

Each of the operators can only be instantiated if the following requirements are
met:

- The indicated operator can be applied to type `T`.
- The result value can be unambiguously converted to `bool`.

The function can be implemented with the return type different from
`std::valarray`. In this case, the replacement type has the following
properties:

- All const member functions of `std::valarray` are provided.
- `std::valarray`, `std::slice_array`, `std::gslice_array`, `std::mask_array`
  and `std::indirect_array` can be constructed from the replacement type.
- For every function taking a const std::valarray<T>& except `begin()` and
  `end()`(since C++11), identical functions taking the replacement types shall
  be added;
- For every function taking two const std::valarray<T>& arguments, identical
  functions taking every combination of const std::valarray<T>& and replacement
  types shall be added.
- The return type does not add more than two levels of template nesting over the
  most deeply-nested argument type.

### Example

```cpp
#include <iostream>
#include <valarray>

int main()
{
    // zero all negatives in a valarray
    std::valarray<int> v = {1, -1, 0, -3, 10, -1, -2};
    std::cout << "Before: ";
    for (auto n : v)
        std::cout << n << ' ';
    std::cout << '\n';
    v[v < 0] = 0;
    std::cout << "After: ";
    for (auto n : v)
        std::cout << n << ' ';
    std::cout << '\n';

    // convert the valarray<bool> result of == to a single bool
    std::valarray<int> a = {1, 2, 3};
    std::valarray<int> b = {2, 4, 6};

    std::cout << "2*a == b is " << std::boolalpha
              << (2 * a == b).min() << '\n';
}
```

Output:

```text
Before: 1 -1 0 -3 10 -1 -2
After: 1 0 0 0 10 0 0
2*a == b is true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3074 | C++98 | `T` is deduced from both the scalar and the `valarray` for
      (2,3), disallowing mixed-type calls | only deduce `T` from the `valarray`

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/operator_cmp*
