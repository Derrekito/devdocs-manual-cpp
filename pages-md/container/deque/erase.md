# std::deque<T,Allocator>::erase

```cpp
iterator erase( iterator pos );  // (until C++11)
iterator erase( const_iterator pos );  // (since C++11)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11)
```

Erases the specified elements from the container.

1) Removes the element at `pos`.

2) Removes the elements in the range `[``first``,``last``)`.

All iterators and references are invalidated, unless the erased elements are at
the end or at the beginning of the container, in which case only the iterators
and references to the erased elements are invalidated. The `end()` iterator is
also invalidated unless the erased elements are at the beginning of the
container and the last element is not erased.

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
elements erased, the number of calls to the assignment operator of T is no more
than the lesser of the number of elements before the erased elements and the
number of elements after the erased elements.

### Notes

When container elements need to be erased based on a predicate, rather than
iterating the container and calling unary `erase`, the iterator range overload
is generally used with `std::remove()/std::remove_if()` to minimise the number
of moves of the remaining (non-removed) elements, this is the erase-remove
idiom. `std::erase_if()` replaces the erase-remove idiom.(since C++20)

### Example

```cpp
#include <deque>
#include <iostream>

void print_container(const std::deque<int>& c)
{
    for (int i : c)
        std::cout << i << ' ';
    std::cout << '\n';
}

int main()
{
    std::deque<int> c{0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    print_container(c);

    c.erase(c.begin());
    print_container(c);

    c.erase(c.begin() + 2, c.begin() + 5);
    print_container(c);

    // Erase all even numbers
    for (std::deque<int>::iterator it = c.begin(); it != c.end();)
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
      behavior of clearing an empty `deque` undefined | not required if `first
      == last`
  LWG 638 | C++98 | the past-the-end iterator was not invalidated | it is
      invalidated if the elements are erased from the middle or the end

### See also

- **erase(std::deque)erase_if(std::deque) (C++20)** — erases all elements
  satisfying specific criteria (function template)
- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/deque/erase*
