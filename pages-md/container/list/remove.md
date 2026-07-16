# std::list<T,Allocator>::remove, remove_if

```cpp
void remove( const T& value );  // (until C++20)
size_type remove( const T& value );  // (since C++20)
template< class UnaryPredicate >
void remove_if( UnaryPredicate p );  // (until C++20)
template< class UnaryPredicate >
size_type remove_if( UnaryPredicate p );  // (since C++20)
```

Removes all elements satisfying specific criteria. Invalidates only the
iterators and references to the removed elements.

1) Removes all elements that are equal to `value` (using `operator==`).

2) Removes all elements for which predicate `p` returns `true`.

### Parameters

- **value** — value of the elements to remove
- **p** — unary predicate which returns ​`true` if the element should be
  removed. The expression `p(v)` must be convertible to `bool` for every
  argument `v` of type (possibly const) `T`, regardless of value category, and
  must not modify `v`. Thus, a parameter type of `T&`is not allowed, nor is `T`
  unless for `T` a move is equivalent to a copy(since C++11). ​

**Type requirements**

**-`UnaryPredicate` must meet the requirements of Predicate.**

### Return value

(none)
*(until C++20)*

The number of elements removed.
*(since C++20)*

### Complexity

Given \(\scriptsize N\)N as `std::distance(begin(), end())`:

1) Exactly \(\scriptsize N\)N comparisons using `operator==`.

2) Exactly \(\scriptsize N\)N applications of the predicate `p`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_list_remove_return_type` | 201806L | (C++20) | Change the return
      type

### Example

```cpp
#include <list>
#include <iostream>

int main()
{
    std::list<int> l = {1, 100, 2, 3, 10, 1, 11, -1, 12};

    auto count1 = l.remove(1);
    std::cout << count1 << " elements equal to 1 were removed\n";

    auto count2 = l.remove_if([](int n){ return n > 10; });
    std::cout << count2 << " elements greater than 10 were removed\n";

    std::cout << "Finally, the list contains: ";
    for (int n : l)
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
2 elements equal to 1 were removed
3 elements greater than 10 were removed
Finally, the list contains: 2 3 10 -1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1207 | C++98 | it was unclear whether iterators and/or references will be
      invalidated | only invalidates iterators and references to the removed
      elements

### See also

- **removeremove_if** — removes elements satisfying specific criteria (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/list/remove*
