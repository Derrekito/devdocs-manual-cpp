# std::gslice_array<T>::gslice_array

```cpp
private:
gslice_array();  // (until C++11)
public:
gslice_array() = delete;  // (since C++11)
public:
gslice_array( const gslice_array& other );  // (2)
```

1) The default constructor is declared private and not defined(until
   C++11)explicitly defined as deleted(since C++11): `gslice_array` is not
   DefaultConstructible.

2) Constructs a `gslice_array` from another `gslice_array` `other`.

### Parameters

- **other** — `gslice_array` to initialize with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 253 | C++98 | the copy constructor was private | made public

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/gslice_array/gslice_array*
