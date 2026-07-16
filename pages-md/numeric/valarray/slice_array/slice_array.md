# std::slice_array<T>::slice_array

```cpp
private:
slice_array();  // (until C++11)
public:
slice_array() = delete;  // (since C++11)
public:
slice_array( const slice_array& other );  // (2)
```

1) The default constructor is declared private and not defined(until
   C++11)explicitly defined as deleted(since C++11): `slice_array` is not
   DefaultConstructible.

2) Constructs a `slice_array` from another `slice_array` `other`.

### Parameters

- **other** — `slice_array` to initialize with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 253 | C++98 | the copy constructor was private | made public

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/slice_array/slice_array*
