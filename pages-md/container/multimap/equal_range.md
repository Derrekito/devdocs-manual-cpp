# std::multimap<Key,T,Compare,Allocator>::equal_range

```cpp
std::pair<iterator,iterator> equal_range( const Key& key );  // (1)
std::pair<const_iterator,const_iterator> equal_range( const Key& key ) const;  // (2)
template< class K >
std::pair<iterator,iterator> equal_range( const K& x );  // (3) (since C++14)
template< class K >
std::pair<const_iterator,const_iterator> equal_range( const K& x ) const;  // (4) (since C++14)
```

Returns a range containing all elements with the given key in the container. The
range is defined by two iterators, one pointing to the first element that is
*not less* than `key` and another pointing to the first element *greater* than
`key`. Alternatively, the first iterator may be obtained with `lower_bound()`,
and the second with `upper_bound()`.

1,2) Compares the keys to `key`.

3,4) Compares the keys to the value `x`. This overload participates in overload
   resolution only if the qualified-id `Compare::is_transparent` is valid and
   denotes a type. It allows calling this function without constructing an
   instance of `Key`.

### Parameters

- **key** — key value to compare the elements to
- **x** — alternative value that can be compared to `Key`

### Return value

`std::pair` containing a pair of iterators defining the wanted range: the first
pointing to the first element that is not *less* than `key` and the second
pointing to the first element *greater* than `key`.

If there are no elements *not less* than `key`, past-the-end (see `end()`)
iterator is returned as the first element. Similarly if there are no elements
*greater* than `key`, past-the-end iterator is returned as the second element.

Since `emplace` and unhinted `insert` always insert at the upper bound, the
order of equivalent elements in the equal range is the order of insertion unless
hinted `insert` or `emplace_hint` was used to insert an element at a different
position.
*(since C++11)*

### Complexity

Logarithmic in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_associative_lookup` | 201304L | (C++14) | Heterogeneous
      comparison lookup, (3,4)

### Example

```cpp
#include <iostream>
#include <map>

int main()
{
    std::multimap<int, char> dict
    {
        {1, 'A'},
        {2, 'B'},
        {2, 'C'},
        {2, 'D'},
        {4, 'E'},
        {3, 'F'}
    };

    auto range = dict.equal_range(2);

    for (auto i = range.first; i != range.second; ++i)
        std::cout << i->first << ": " << i->second << '\n';
}
```

Output:

```text
2: B
2: C
2: D
```

### See also

- **find** — finds element with specific key (public member function)
- **contains (C++20)** — checks if the container contains element with specific
  key (public member function)
- **count** — returns the number of elements matching specific key (public
  member function)
- **upper_bound** — returns an iterator to the first element *greater* than the
  given key (public member function)
- **lower_bound** — returns an iterator to the first element *not less* than the
  given key (public member function)
- **equal_range** — returns range of elements matching a specific key (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/equal_range*
