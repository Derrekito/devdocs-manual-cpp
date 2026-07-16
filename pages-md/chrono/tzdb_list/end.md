# std::chrono::tzdb_list::end, std::chrono::tzdb_list::cend

```cpp
const_iterator end() const noexcept;  // (since C++20)
const_iterator cend() const noexcept;  // (since C++20)
```

Returns the past-the-end iterator of the `tzdb_list`. Attempting to dereference
this iterator results in undefined behavior.

### Return value

The past-the-end iterator.

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb_list/end*
