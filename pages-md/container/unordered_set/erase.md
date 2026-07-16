# std::unordered_set<Key,Hash,KeyEqual,Allocator>::erase

```cpp
iterator erase( iterator pos );  // (since C++11) (until C++23)
iterator erase( iterator pos ) requires(!std::same_as<iterator, const_iterator>);  // (since C++23)
iterator erase( const_iterator pos );  // (2) (since C++11)
iterator erase( const_iterator first, const_iterator last );  // (3) (since C++11)
size_type erase( const Key& key );  // (4) (since C++11)
template< class K >
size_type erase( K&& x );  // (5) (since C++23)
```

Removes specified elements from the container. The order of the remaining
elements is preserved. (This makes it possible to erase individual elements
while iterating through the container.)

1,2) Removes the element at `pos`. Only one overload is provided if `iterator`
   and `const_iterator` are the same type.

3) Removes the elements in the range `[``first``,``last``)`, which must be a
   valid range in `*this`.

4) Removes the element (if one exists) with the key equivalent to `key`.

5) Removes all elements with key that compares *equivalent* to the value `x`.
   This overload participates in overload resolution only if
   `Hash::is_transparent` and `KeyEqual::is_transparent` are valid and each
   denotes a type, and neither `iterator` nor `const_iterator` is implicitly
   convertible from `K`. This assumes that such `Hash` is callable with both `K`
   and `Key` type, and that the `KeyEqual` is transparent, which, together,
   allows calling this function without constructing an instance of `Key`.

References and iterators to the erased elements are invalidated. Other iterators
and references are not invalidated.

The iterator `pos` must be valid and dereferenceable. Thus the `end()` iterator
(which is valid, but is not dereferenceable) cannot be used as a value for
`pos`.

### Parameters

- **pos** — iterator to the element to remove
- **first, last** — range of elements to remove
- **key** — key value of the elements to remove
- **x** — a value of any type that can be transparently compared with a key
  denoting the elements to remove

### Return value

1-3) Iterator following the last removed element.

4) Number of elements removed (`0` or `1`).

5) Number of elements removed.

### Exceptions

1-3) Throws nothing.

4,5) Any exceptions thrown by the `Hash` and `KeyEqual` object.

### Complexity

Given an instance `c` of `unordered_set`:

1,2) Average case: constant, worst case: `c.size()`.

3) Average case: `std::distance(first, last)`, worst case: `c.size()`.

4) Average case: `c.count(key)`, worst case: `c.size()`.

5) Average case: `c.count(x)`, worst case: `c.size()`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_associative_heterogeneous_erasure` | 202110L | (C++23) |
      Heterogeneous erasure in associative containers and unordered associative
      containers; overload (5)

### Example

```cpp
#include <unordered_set>
#include <iostream>

int main()
{
    std::unordered_set<int> c = {1, 2, 3, 4, 1, 2, 3, 4};

    auto print = [&c]
    {
        std::cout << "c = { ";
        for (int n : c)
            std::cout << n << ' ';
        std::cout << "}\n";
    };
    print();

    std::cout << "Erase all odd numbers:\n";
    for (auto it = c.begin(); it != c.end();)
    {
        if (*it % 2 != 0)
            it = c.erase(it);
        else
            ++it;
    }
    print();

    std::cout << "Erase 1, erased count: " << c.erase(1) << '\n';
    std::cout << "Erase 2, erased count: " << c.erase(2) << '\n';
    std::cout << "Erase 2, erased count: " << c.erase(2) << '\n';
    print();
}
```

Possible output:

```text
c = { 1 2 3 4 }
Erase all odd numbers:
c = { 2 4 }
Erase 1, erased count: 0
Erase 2, erased count: 1
Erase 2, erased count: 0
c = { 4 }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2059 | C++11 | there was ambiguity for overload (2) | added overload (1)
  LWG 2356 | C++11 | the order of non-equivalent elements that are not erased
      was not guaranteed to be preserved | required to be preserved

### See also

- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/erase*
