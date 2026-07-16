# std::declare_no_pointers

```cpp
void declare_no_pointers( char *p, std::size_t n );  // (since C++11) (removed in C++23)
```

Informs the garbage collector or leak detector that the specified memory region
(`n` bytes beginning at the byte pointed to by `p`) contains no traceable
pointers. If any part of the region is within an allocated object, the entire
region must be contained in the same object.

### Parameters

- **p** — pointer to the beginning of the range
- **n** — the number of bytes in the range

### Return value

(none)

### Exceptions

Throws nothing.

### See also

- **undeclare_no_pointers (C++11)(removed in C++23)** — cancels the effect of
  `std::declare_no_pointers` (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/gc/declare_no_pointers*
