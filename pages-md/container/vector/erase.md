# std::vector<T,Allocator>::erase

```cpp
iterator erase( iterator pos );  // (until C++11)
iterator erase( const_iterator pos );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator pos );  // (since C++20)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator first, const_iterator last );  // (since C++20)
```

Erases the specified elements from the container.

1) Removes the element at `pos`.

2) Removes the elements in the range `[``first``,``last``)`.

Iterators (including the `end()` iterator) and references to the elements at or
after the point of the erase are invalidated.

The iterator `pos` must be valid and dereferenceable. Thus the `end()` iterator
(which is valid, but is not dereferenceable) cannot be used as a value for
`pos`.

The iterator `first` does not need to be dereferenceable if `first == last`:
erasing an empty range is a no-op.

### Parameters

- **pos** — iterator to the element to remove
- **first, last** — range of elements to remove

**Type requirements**

**-`T` must meet the requirements of MoveAssignable.**

### Return value

Iterator following the last removed element.

1) If `pos` refers to the last element, then the `end()` iterator is returned.

2) If `last == end()` prior to removal, then the updated `end()` iterator is
   returned.

If `[``first``,``last``)` is an empty range, then `last` is returned.

### Exceptions

Does not throw unless an exception is thrown by the assignment operator of `T`.

### Complexity

Linear: the number of calls to the destructor of T is the same as the number of
elements erased, the assignment operator of T is called the number of times
equal to the number of elements in the vector after the erased elements.

### Notes

When container elements need to be erased based on a predicate, rather than
iterating the container and calling unary `erase`, the iterator range overload
is generally used with `std::remove()/std::remove_if()` to minimise the number
of moves of the remaining (non-removed) elements, this is the erase-remove
idiom. `std::erase_if()` replaces the erase-remove idiom.(since C++20)

### Example

```cpp
#include <vector>
#include <iostream>

void print_container(const std::vector<int>& c)
{
    for (int i : c)
        std::cout << i << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> c{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    print_container(c);

    c.erase(c.begin());
    print_container(c);

    c.erase(c.begin() + 2, c.begin() + 5);
    print_container(c);

    // Erase all even numbers
    for (std::vector<int>::iterator it = c.begin(); it != c.end();)
    {
        if (*it % 2 == 0)
            it = c.erase(it);
        else
            ++it;
    }
    print_container(c);
}
```

Output:

```text
0 1 2 3 4 5 6 7 8 9
1 2 3 4 5 6 7 8 9
1 2 6 7 8 9
1 7 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 151 | C++98 | `first` was required to be dereferenceable, which made the
      behavior of clearing an empty `vector` undefined | not required if `first
      == last`
  LWG 414 | C++98 | iterators at the point of erase were not invalidated | they
      are also invalidated

### See also

- **erase(std::vector)erase_if(std::vector) (C++20)** — erases all elements
  satisfying specific criteria (function template)
- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/erase*
