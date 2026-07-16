# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::at

```cpp
T& at( const Key& key );  // (1) (since C++11)
const T& at( const Key& key ) const;  // (2) (since C++11)
```

Returns a reference to the mapped value of the element with key equivalent to
`key`. If no such element exists, an exception of type `std::out_of_range` is
thrown.

### Parameters

- **key** — the key of the element to find

### Return value

Reference to the mapped value of the requested element.

### Exceptions

`std::out_of_range` if the container does not have an element with the specified
`key`.

### Complexity

Average case: constant, worst case: linear in size.

### See also

- **operator[]** — access or insert specified element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/at*
