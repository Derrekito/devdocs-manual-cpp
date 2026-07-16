# std::map<Key,T,Compare,Allocator>::value_compare

```cpp
class value_compare;
```

`std::map::value_compare` is a function object that compares objects of type
`std::map::value_type` (key-value pairs) by comparing of the first components of
the pairs.

### Member types

- **`result_type` (deprecated in C++17)(removed in C++20)** — bool
- **`first_argument_type` (deprecated in C++17)(removed in C++20)** —
  `value_type`
- **`second_argument_type` (deprecated in C++17)(removed in C++20)** —
  `value_type`

These member types are obtained via publicly inheriting
std::binary_function<value_type, value_type, bool>.
*(until C++11)*

### Protected member objects

- **Compare comp** — the stored comparator (protected member object)

### Member functions

- ****(constructor)**** — constructs a new `value_compare` object (protected
  member function)
- ****operator()**** — compares two values of type `value_type` (public member
  function)

## std::map<Key,T,Compare,Alloc>::value_compare::value_compare

```cpp
protected:
value_compare( Compare c );
```

Initializes the internal instance of the comparator to `c`.

### Parameters

- **c** — comparator to assign

## std::map<Key,T,Compare,Alloc>::value_compare::operator()

```cpp
bool operator()( const value_type& lhs, const value_type& rhs ) const;
```

Compares `lhs.first` and `rhs.first` by calling the stored comparator.

### Parameters

- **lhs, rhs** — values to compare

### Return value

`comp(lhs.first, rhs.first)`

### Exceptions

May throw implementation-defined exceptions.

---
*Source: https://en.cppreference.com/w/cpp/container/map/value_compare*
