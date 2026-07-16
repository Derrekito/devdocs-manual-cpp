# operator<,<=,>,>=(std::basic_const_iterator<Iter>)

```cpp
template< /*not-a-const-iterator*/ I >
friend constexpr bool operator<( const I& x, const basic_const_iterator& y )
    requires std::random_access_iterator<Iter> && std::totally_ordered_with<Iter, I>;  // (1) (since C++23)
template< /*not-a-const-iterator*/ I >
friend constexpr bool operator>( const I& x, const basic_const_iterator& y )
    requires std::random_access_iterator<Iter> && std::totally_ordered_with<Iter, I>;  // (2) (since C++23)
template< /*not-a-const-iterator*/ I >
friend constexpr bool operator<=( const I& x, const basic_const_iterator& y )
    requires std::random_access_iterator<Iter> && std::totally_ordered_with<Iter, I>;  // (3) (since C++23)
template< /*not-a-const-iterator*/ I >
friend constexpr bool operator>=( const I& x, const basic_const_iterator& y )
    requires std::random_access_iterator<Iter> && std::totally_ordered_with<Iter, I>;  // (4) (since C++23)
```

Compare a `basic_const_iterator` with another value. These function templates
are used when the left operand is not a `basic_const_iterator`.

`I` satisfies the exposition-only concept `/*not-a-const-iterator*/` if and only
if it's not a specialization of `basic_const_iterator`.

These functions are not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `basic_const_iterator<Iter>`
is an associated class of the arguments.

### Parameters

- **x, y** — iterators to compare

### Return value

1) `x < y.base()`

2) `x > y.base()`

3) `x <= y.base()`

4) `x >= y.base()`

### Notes

If the left operand is a `basic_const_iterator`, the member comparison functions
are used.

### Example

```cpp
#include <iterator>

int main()
{
    static int arr[1];
    static constexpr std::basic_const_iterator<int*> it = std::end(arr);
    static_assert(arr < it);
}
```

### See also

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/operator_cmp2*
