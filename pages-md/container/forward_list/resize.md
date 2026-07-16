# std::forward_list<T,Allocator>::resize

```cpp
void resize( size_type count );  // (1) (since C++11)
void resize( size_type count, const value_type& value );  // (2) (since C++11)
```

Resizes the container to contain `count` elements, does nothing if `count ==
std::distance(begin(), end())` (i.e. if `count` is equal to the current size).

If the current size is greater than `count`, the container is reduced to its
first `count` elements.

If the current size is less than `count`,

1) additional default-inserted elements are appended.

2) additional copies of `value` are appended.

### Parameters

- **count** — new size of the container
- **value** — the value to initialize the new elements with

**Type requirements**

**-`T` must meet the requirements of DefaultInsertable in order to use overload (1).**

**-`T` must meet the requirements of CopyInsertable in order to use overload (2).**

### Return value

(none)

### Complexity

Linear in the difference between the current size and `count`. Additional
complexity possible due to list traversal to reach the first element to
erase/the end position to insert.

### Notes

If value-initialization in overload (1) is undesirable, for example, if the
elements are of non-class type and zeroing out is not needed, it can be avoided
by providing a custom `Allocator::construct`.

### Example

```cpp
#include <forward_list>
#include <iostream>

void print(auto rem, const std::forward_list<int>& c)
{
    for (std::cout << rem; const int el : c)
        std::cout << el << ' ';
    std::cout << '\n';
}

int main()
{
    std::forward_list<int> c = {1, 2, 3};
    print("The forward_list holds: ", c);

    c.resize(5);
    print("After resize up to 5: ", c);

    c.resize(2);
    print("After resize down to 2: ", c);

    c.resize(6, 4);
    print("After resize up to 6 (initializer = 4): ", c);
}
```

Output:

```text
The forward_list holds: 1 2 3
After resize up to 5: 1 2 3 0 0
After resize down to 2: 1 2
After resize up to 6 (initializer = 4): 1 2 4 4 4 4
```

### See also

- **insert_after** — inserts elements after an element (public member function)
- **erase_after** — erases an element after an element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/forward_list/resize*
