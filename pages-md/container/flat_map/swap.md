# std::flat_map<Key,T,Compare,KeyContainer,MappedContainer>::swap

```cpp
void swap( flat_map& other ) noexcept;  // (since C++23)
```

Exchanges the contents of the container adaptor with those of `other`.
Effectively calls

```cpp
ranges::swap(compare, other.compare);
ranges::swap(c.keys, other.c.keys);
ranges::swap(c.values, other.c.values);
```

### Parameters

- **other** — container adaptor to exchange the contents with

### Return value

(none)

### Exceptions

(none)

### Complexity

Same as underlying container (typically constant).

### Example

### See also

- **std::swap(std::flat_map) (C++23)** — specializes the `std::swap` algorithm
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/flat_map/swap*
