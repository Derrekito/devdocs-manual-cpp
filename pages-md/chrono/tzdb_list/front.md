# std::chrono::tzdb_list::front

```cpp
const std::chrono::tzdb& front() const noexcept;  // (since C++20)
```

Obtains a reference to the first `std::chrono::tzdb` in the list. Simultaneous
calls to this function and `std::chrono::reload_tzdb()` does not introduce a
data race.

### Return value

A reference to the first `std::chrono::tzdb` in the list.

---
*Source: https://en.cppreference.com/w/cpp/chrono/tzdb_list/front*
