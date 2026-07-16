# std::locale::operator=

```cpp
const locale& operator=( const locale& other ) throw();  // (until C++11)
const locale& operator=( const locale& other ) noexcept;  // (since C++11)
```

Creates a copy of `other`, replacing the contents of `*this`. The reference
counts of all facets held by `other` are incremented. The reference counts of
all facets previously held by `*this` are decremented, and those facets whose
reference count becomes zero are deleted.

### Return value

Returns `*this`, which is now a copy of `other`.

### Example

### See also

- **(constructor)** — constructs a new locale (public member function)

---
*Source: https://en.cppreference.com/w/cpp/locale/locale/operator%3D*
