# std::map<Key,T,Compare,Allocator>::at

```cpp
T& at( const Key& key );  // (1)
const T& at( const Key& key ) const;  // (2)
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

Logarithmic in the size of the container.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 464 | C++98 | `map` did not have this member function | added
  LWG 703 | C++98 | the complexity requirement was missing | added
  LWG 2007 | C++98 | the return value referred to the requested element | refers
      to its mapped value

### See also

- **operator[]** — access or insert specified element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/map/at*
