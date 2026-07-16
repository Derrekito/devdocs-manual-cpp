# std::gslice_array<T>::operator=

```cpp
void operator=( const T& value ) const;  // (1)
void operator=( const std::valarray<T>& val_arr ) const;  // (2)
const gslice_array& operator=( const gslice_array& other_arr ) const;  // (3)
```

Assigns values to all referred elements.

1) Assigns `value` to all of the elements.

2) Assigns the elements of `val_arr` to the referred to elements of `*this`.

3) Assigns the selected elements from `other_arr` to the referred to elements of
   `*this`.

### Parameters

- **value** — a value to assign to all of the referred elements
- **val_arr** — `std::valarray` to assign
- **other_arr** — `std::gslice_array` to assign

### Return value

1,2) (none)

3) `*this`

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 123 | C++98 | overload (2) was non-const | made const
  LWG 253 | C++98 | the copy assignment operator was private | made public
  LWG 621 | C++98 | the copy assignment operator was non-const | made const

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/gslice_array/operator%3D*
