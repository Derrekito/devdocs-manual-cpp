# std::undeclare_no_pointers

```cpp
void undeclare_no_pointers( char *p, std::size_t n );  // (since C++11) (removed in C++23)
```

Unregisters a range earlier registered with `std::declare_no_pointers()`.

### Parameters

- **p** — pointer to the beginning of the range previously registered with
  `std::declare_no_pointers`
- **n** — the number of bytes in the range, same value as previously used with
  `std::declare_no_pointers`

### Return value

(none)

### Exceptions

Throws nothing.

### See also

- **declare_no_pointers (C++11)(removed in C++23)** — declares that a memory
  area does not contain traceable pointers (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/gc/undeclare_no_pointers*
