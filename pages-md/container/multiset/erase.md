# std::multiset<Key,Compare,Allocator>::erase

```cpp
iterator erase( iterator pos );  // (until C++23)
iterator erase( iterator pos ) requires(!std::same_as<iterator, const_iterator>);  // (since C++23)
iterator erase( const_iterator pos );  // (2) (since C++11)
iterator erase( iterator first, iterator last );  // (until C++11)
iterator erase( const_iterator first, const_iterator last );  // (since C++11)
size_type erase( const Key& key );  // (4)
template< class K >
size_type erase( K&& x );  // (5) (since C++23)
```

Removes specified elements from the container. The order of the remaining
equivalent elements is preserved.

1,2) Removes the element at `pos`. Only one overload is provided if `iterator`
   and `const_iterator` are the same type.(since C++11)

3) Removes the elements in the range `[``first``,``last``)`, which must be a
   valid range in `*this`.

4) Removes all elements with the key equivalent to `key`.

5) Removes all elements with key that compares *equivalent* to the value `x`.
   This overload participates in overload resolution only if the qualified-id
   `Compare::is_transparent` is valid and denotes a type, and neither `iterator`
   nor `const_iterator` is implicitly convertible from `K`. It allows calling
   this function without constructing an instance of `Key`.

References and iterators to the erased elements are invalidated. Other
references and iterators are not affected.

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

4) Number of elements removed.

5) Number of elements removed.

### Exceptions

1-3) Throws nothing.

4,5) Any exceptions thrown by the `Compare` object.

### Complexity

Given an instance `c` of `multiset`:

1,2) Amortized constant

3) `log(c.size()) + std::distance(first, last)`

4) `log(c.size()) + c.count(key)`

5) `log(c.size()) + c.count(x)`

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_associative_heterogeneous_erasure` | 202110L | (C++23) |
      Heterogeneous erasure in associative containers and unordered associative
      containers; overload (5)

### Example

```cpp
#include <set>
#include <iostream>

int main()
{
    std::multiset<int> c = {1, 2, 3, 4, 1, 2, 3, 4};

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

Output:

```text
c = { 1 1 2 2 3 3 4 4 }
Erase all odd numbers:
c = { 2 2 4 4 }
Erase 1, erased count: 0
Erase 2, erased count: 2
Erase 2, erased count: 0
c = { 4 4 }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 130 | C++98 | the return type of overloads (1) and (3) was void (it is not
      consistent with the `erase()` requirement on sequence containers) |
      corrected to `iterator`
  LWG 371 | C++98 | the order of equivalent elements that are not erased was not
      guaranteed to be preserved | required to be preserved
  LWG 2059 | C++11 | replacing overload (1) with overload (2) introduced new
      ambiguity | added overload (1) back

### See also

- **clear** — clears the contents (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multiset/erase*
