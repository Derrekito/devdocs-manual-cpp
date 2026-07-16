# std::multimap<Key,T,Compare,Allocator>::lower_bound

```cpp
iterator lower_bound( const Key& key );  // (1)
const_iterator lower_bound( const Key& key ) const;  // (2)
template< class K >
iterator lower_bound( const K& x );  // (3) (since C++14)
template< class K >
const_iterator lower_bound( const K& x ) const;  // (4) (since C++14)
```

1,2) Returns an iterator pointing to the first element that is *not less* than
   (i.e. greater or equal to) `key`.

3,4) Returns an iterator pointing to the first element that compares *not less*
   (i.e. greater or equal) to the value `x`. This overload participates in
   overload resolution only if the qualified-id `Compare::is_transparent` is
   valid and denotes a type. It allows calling this function without
   constructing an instance of `Key`.

### Parameters

- **key** — key value to compare the elements to
- **x** — alternative value that can be compared to `Key`

### Return value

Iterator pointing to the first element that is not *less* than `key`. If no such
element is found, a past-the-end iterator (see `end()`) is returned.

### Complexity

Logarithmic in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_associative_lookup` | 201304L | (C++14) | Heterogeneous
      comparison lookup in associative containers, for overloads (3,4)

### Example

### See also

- **equal_range** — returns range of elements matching a specific key (public
  member function)
- **upper_bound** — returns an iterator to the first element *greater* than the
  given key (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/lower_bound*
