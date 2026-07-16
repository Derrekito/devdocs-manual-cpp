# std::indirect_array<T>::indirect_array

```cpp
private:
indirect_array();  // (until C++11)
public:
indirect_array() = delete;  // (since C++11)
public:
indirect_array( const indirect_array& other );  // (2)
```

1) The default constructor is declared private and not defined(until
   C++11)explicitly defined as deleted(since C++11): `indirect_array` is not
   DefaultConstructible.

2) Constructs a `indirect_array` from another `indirect_array` `other`.

### Parameters

- **other** — `indirect_array` to initialize with

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 253 | C++98 | the copy constructor was private | made public

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/indirect_array/indirect_array*
