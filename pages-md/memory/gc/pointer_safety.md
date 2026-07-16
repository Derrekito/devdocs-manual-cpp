# std::pointer_safety

```cpp
enum class pointer_safety {
     relaxed,
     preferred,
     strict
};  // (since C++11) (removed in C++23)
```

The scoped enumeration type `pointer_safety` lists the pointer safety modes
supported by C++.

### Enumeration constants

- **`pointer_safety::strict`** — Only safely-derived pointers (pointers to
  objects allocated with new or subobjects thereof) may be dereferenced or
  deallocated. Garbage collector may be active.
- **`pointer_safety::preferred`** — All pointers are considered valid and may be
  dereferenced or deallocated. A reachability-based leak detector may be active.
- **`pointer_safety::relaxed`** — All pointers are considered valid and may be
  dereferenced or deallocated.

### See also

- **get_pointer_safety (C++11)(removed in C++23)** — returns the current pointer
  safety model (function)

---
*Source: https://en.cppreference.com/w/cpp/memory/gc/pointer_safety*
