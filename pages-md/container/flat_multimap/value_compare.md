# std::flat_multimap<Key,T,Compare,KeyContainer,MappedContainer>::value_compare

```cpp
class value_compare;  // (since {std})
```

`std::flat_multimap::value_compare` is a function object that compares objects
of type `std::flat_multimap::const_reference` (a pair of references to the
stored key/value) by comparing the first components (i.e. keys) of the passed
pairs.

### Member objects

- **`comp` (private)** — the comparison function object of type
  `std::flat_multimap::key_compare` (exposition-only member object*)

### Member functions

- **(constructor) (private)** — constructs `value_compare` object (public member
  function)
- **operator()** — compares two values of type `value_type` (public member
  function)

## std::flat_multimap<Key,T,Compare,KeyContainer,MappedContainer>::value_compare::value_compare

```cpp
private:
value_compare( key_compare c );  // (exposition only*)
```

Initializes the underlying instance of the comparator `comp` to `c`.

### Parameters

- **c** — a comparator to assign

## std::flat_multimap<Key,T,Compare,KeyContainer,MappedContainer>::value_compare::operator()

```cpp
bool operator()( const const_reference& lhs, const const_reference& rhs ) const;
```

Compares `lhs.first` and `rhs.first` by calling the stored comparator `comp`.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`comp(lhs.first, rhs.first)`

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/container/flat_multimap/value_compare*
