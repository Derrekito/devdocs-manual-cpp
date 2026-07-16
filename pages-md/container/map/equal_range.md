# std::map<Key,T,Compare,Allocator>::equal_range

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
    const std::map<int, const char*> m
    {
        {0, "zero"},
        {1, "one"},
        {2, "two"}
    };

    auto p = m.equal_range(1);
    for (auto& q = p.first; q != p.second; ++q)
        std::cout << "m[" << q->first << "] = " << q->second << '\n';

    if (p.second == m.find(2))
        std::cout << "end of equal_range (p.second) is one-past p.first\n";
    else
        std::cout << "unexpected; p.second expected to be one-past p.first\n";

    auto pp = m.equal_range(-1);
    if (pp.first == m.begin())
        std::cout << "pp.first is iterator to first not-less than -1\n";
    else
        std::cout << "unexpected pp.first\n";

    if (pp.second == m.begin())
        std::cout << "pp.second is iterator to first element greater-than -1\n";
    else
        std::cout << "unexpected pp.second\n";

    auto ppp = m.equal_range(3);
    if (ppp.first == m.end())
        std::cout << "ppp.first is iterator to first not-less than 3\n";
    else
        std::cout << "unexpected ppp.first\n";

    if (ppp.second == m.end())
        std::cout << "ppp.second is iterator to first element greater-than 3\n";
    else
        std::cout << "unexpected ppp.second\n";
}
```

Output:

```text
m[1] = one
end of equal_range (p.second) is one-past p.first
pp.first is iterator to first not-less than -1
pp.second is iterator to first element greater-than -1
ppp.first is iterator to first not-less than 3
ppp.second is iterator to first element greater-than 3
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
*Source: https://en.cppreference.com/w/cpp/container/map/equal_range*
