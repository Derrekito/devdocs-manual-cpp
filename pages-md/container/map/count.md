# std::map<Key,T,Compare,Allocator>::count

```cpp
size_type count( const Key& key ) const;  // (1)
template< class K >
size_type count( const K& x ) const;  // (2) (since C++14)
```

Returns the number of elements with key that compares *equivalent* to the
specified argument.

1) Returns the number of elements with key `key`. This is either 1 or 0 since
   this container does not allow duplicates.

2) Returns the number of elements with key that compares equivalent to the value
   `x`. This overload participates in overload resolution only if the
   qualified-id `Compare::is_transparent` is valid and denotes a type. It allows
   calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the elements to count
- **x** — alternative value to compare to the keys

### Return value

Number of elements with key that compares *equivalent* to `key` or `x`, which,
for overload (1), is either 1 or 0.

### Complexity

Logarithmic in the size of the container plus linear in the number of elements
found.

### Example

### See also

- **find** — finds element with specific key (public member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/count*
