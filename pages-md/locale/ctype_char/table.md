# std::ctype<char>::table

```cpp
const mask* table() const throw();  // (until C++11)
const mask* table() const noexcept;  // (since C++11)
```

Returns the classification table that was provided in the constructor of this
instance of `std::ctype<char>`, or returns a copy of `classic_table()` if none
was provided.

### Parameters

(none)

### Return value

A pointer to the first element in the classification table (which an array of
size `std::ctype<char>::table_size`).

### Example

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype_char/table*
