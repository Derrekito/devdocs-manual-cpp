# Standard library header <typeindex> (C++11)

This header is part of the type support library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support

**Classes**

- **type_index (C++11)** — wrapper around a `type_info` object, that can be used
  as index in associative and unordered associative containers (class)
- **std::hash<std::type_index> (C++11)** — hash support for `std::type_index`
  (class template specialization)

**Forward declarations**

- **hash (C++11)** — hash function object (class template)

### Synopsis

```cpp
#include <compare>

namespace std {
  class type_index;
  template<class T> struct hash;
  template<> struct hash<type_index>;
}
```

#### Class `std::type_index`

```cpp
namespace std {
  class type_index {
  public:
    type_index(const type_info& rhs) noexcept;
    bool operator==(const type_index& rhs) const noexcept;
    bool operator< (const type_index& rhs) const noexcept;
    bool operator> (const type_index& rhs) const noexcept;
    bool operator<=(const type_index& rhs) const noexcept;
    bool operator>=(const type_index& rhs) const noexcept;
    strong_ordering operator<=>(const type_index& rhs) const noexcept;
    size_t hash_code() const noexcept;
    const char* name() const noexcept;

  private:
    const type_info* target; // exposition only
    // Note that the use of a pointer here, rather than a reference,
    // means that the default copy/move constructor and assignment
    // operators will be provided and work as expected.
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/typeindex*
