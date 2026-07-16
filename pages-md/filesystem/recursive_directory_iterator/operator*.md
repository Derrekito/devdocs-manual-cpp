# std::filesystem::recursive_directory_iterator::operator*, std::filesystem::recursive_directory_iterator::operator->

```cpp
const std::filesystem::directory_entry& operator*() const;  // (1) (since C++17)
const std::filesystem::directory_entry* operator->() const;  // (2) (since C++17)
```

Accesses the pointed-to `directory_entry`.

The result of `operator*` or `operator->` on the end iterator is undefined
behavior.

### Parameters

(none)

### Return value

1) Value of the `directory_entry` referred to by this iterator.

2) Pointer to the `directory_entry` referred to by this iterator.

### Exceptions

May throw implementation-defined exceptions.

### See also

- **operator*operator->** — accesses the pointed-to entry (public member
  function of `std::filesystem::directory_iterator`)

---
*Source: https://en.cppreference.com/w/cpp/filesystem/recursive_directory_iterator/operator**
