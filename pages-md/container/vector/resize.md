# std::vector<T,Allocator>::resize

```cpp
void resize( size_type count );  // (1) (constexpr since C++20)
void resize( size_type count, const value_type& value );  // (2) (constexpr since C++20)
```

Resizes the container to contain `count` elements, does nothing if `count ==
size()`.

If the current size is greater than `count`, the container is reduced to its
first `count` elements.

If the current size is less than `count`,

1) additional default-inserted elements are appended.

2) additional copies of `value` are appended.

### Parameters

- **count** — new size of the container
- **value** — the value to initialize the new elements with

**Type requirements**

**-`T` must meet the requirements of MoveInsertable and DefaultInsertable in order to use overload (1).**

**-`T` must meet the requirements of CopyInsertable in order to use overload (2).**

### Return value

(none)

### Complexity

Linear in the difference between the current size and `count`. Additional
complexity possible due to reallocation if capacity is less than `count`.

### Exceptions

If an exception is thrown for any reason, these functions have no effect (strong
exception safety guarantee). Although not explicitly specified,
`std::length_error` is thrown if the capacity required by the new vector would
exceed `max_size()`.

In overload (1), if `T`'s move constructor is not noexcept and T is not
CopyInsertable into `*this`, vector will use the throwing move constructor. If
it throws, the guarantee is waived and the effects are unspecified.
*Template:mark since 11*

### Notes

If value-initialization in overload (1) is undesirable, for example, if the
elements are of non-class type and zeroing out is not needed, it can be avoided
by providing a custom `Allocator::construct`.
Vector capacity is never reduced when resizing to smaller size because that
would invalidate all iterators, rather than only the ones that would be
invalidated by the equivalent sequence of `pop_back()` calls.

### Example

```cpp
#include <vector>
#include <iostream>

void print(auto rem, const std::vector<int>& c)
{
    for (std::cout << rem; const int el : c)
        std::cout << el << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> c = {1, 2, 3};
    print("The vector holds: ", c);

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
The vector holds: 1 2 3
After resize up to 5: 1 2 3 0 0
After resize down to 2: 1 2
After resize up to 6 (initializer = 4): 1 2 4 4 4 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 679 | C++98 | `resize()` passed `value` by value | passes by const
      reference
  LWG 1525 | C++98 | the behavior of `resize(size())` was not specified |
      specified
  LWG 2033 | C++11 | 1. elements were removed by using `erase()` 2. the type
      requirements of `T` were incorrect | 1. uses `pop_back()` 2. corrected
  LWG 2066 | C++11 | overload (1) did not have the exception safety guarantee of
      overload (2) | added

### See also

- **size** — returns the number of elements (public member function)
- **insert** — inserts elements (public member function)
- **erase** — erases elements (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/vector/resize*
