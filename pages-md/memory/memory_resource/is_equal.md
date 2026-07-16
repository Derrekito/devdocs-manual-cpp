# std::pmr::memory_resource::is_equal

```cpp
bool is_equal( const memory_resource& other ) const noexcept;  // (since C++17)
```

Compares `*this` for equality with `other`. Two `memory_resource`s compare equal
if and only if memory allocated from one `memory_resource` can be deallocated
from the other and vice versa.

Equivalent to `return do_is_equal(other);`.

### See also

- **do_is_equal [virtual]** — compare for equality with another
  `memory_resource` (virtual private member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/is_equal*
