# std::ctype<char>::classic_table

```cpp
static const mask* classic_table() throw();  // (until C++11)
static const mask* classic_table() noexcept;  // (since C++11)
```

Returns the classification table that matches the classification used by the
minimal "C" locale.

### Parameters

(none)

### Return value

A pointer to the first element in the classification table (which is an array of
size `std::ctype<char>::table_size`).

### Notes

Default-constructed `std::ctype<char>` facets use this table for classification.

### Example

---
*Source: https://en.cppreference.com/w/cpp/locale/ctype_char/classic_table*
