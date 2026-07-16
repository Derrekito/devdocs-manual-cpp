# std::vector<T,Allocator>::erase

Removes the element at `pos`, or the range `[first,last)`, moving every
later element down to fill the gap. Returns an iterator to the element
that followed the removed one(s) — reassign your loop iterator to it to
keep iterating correctly. Iterators and references at or after the erase
point are invalidated (the vector shrinks; nothing reallocates).

```cpp skip
it = v.erase(pos);          // remove one element
it = v.erase(first, last);  // remove a [first,last) range
```

### What you provide

- **pos** — iterator to the element to remove; must be dereferenceable
  (`end()` doesn't qualify).
- **first, last** — the range to remove; `first` need not be
  dereferenceable when `first == last` (erasing an empty range is a
  no-op).

### Guarantees and costs

- Linear: the destructor runs once per erased element, and the
  assignment operator runs once per element that remains after the
  erased ones (each is move-assigned down to close the gap).
- Iterators and references at or after the erase point are invalidated,
  `end()` included; nothing before the erase point is affected.
- Doesn't throw unless `T`'s assignment operator does.
- Return value: the iterator following the last removed element. If
  `pos` (or `last`) was `end()` before the call, the updated `end()` is
  returned; erasing an empty range returns `last`.

### Gotchas

- After `it = c.erase(it)`, `it` is already advanced — don't also
  `++it`, and don't reuse any *other* iterator you were holding into the
  vector past the erase point.
- Erasing one element at a time in a loop costs O(N) per call, O(N²)
  overall. To remove by predicate, use the erase-remove idiom
  (`std::remove`/`std::remove_if`, then a single range `erase`), or
  `std::erase_if` (C++20), which does both steps in one call.

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

```text
0 1 2 3 4 5 6 7 8 9 
1 2 3 4 5 6 7 8 9 
1 2 6 7 8 9 
1 7 9 
```

### Reference

```cpp skip
iterator erase( iterator pos );  // (until C++11)
iterator erase( const_iterator pos );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator pos );  // (since C++20)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11) (until C++20)
constexpr iterator erase( const_iterator first, const_iterator last );  // (since C++20)
```

Requires `T` to be MoveAssignable. Defect reports: LWG 151 clarified that
`first` need not be dereferenceable when `first == last` (previously
this left clearing an empty vector undefined); LWG 414 clarified that
iterators at the erase point are invalidated (previously unspecified).

### See also

- **erase(vector)** / **erase_if(vector)** (C++20) — erases every
  element matching a value or predicate in one call
- **clear** — erases every element
- **insert** — adds elements

---
*Source: https://en.cppreference.com/w/cpp/container/vector/erase*
