# std::mask_array<T>::mask_array

```cpp
private:
mask_array();  // (until C++11)
public:
mask_array() = delete;  // (since C++11)
public:
mask_array( const mask_array& other );  // (2)
```

1) The default constructor is declared private and not defined(until
   C++11)explicitly defined as deleted(since C++11): `mask_array` is not
   DefaultConstructible.

2) Constructs a `mask_array` from another `mask_array` `other`.

### Parameters

- **other** — `mask_array` to initialize with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 253 | C++98 | the copy constructor was private | made public

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/mask_array/mask_array*
