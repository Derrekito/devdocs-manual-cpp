# std::basic_ios<CharT,Traits>::~basic_ios

```cpp
virtual ~basic_ios();
```

Destroys the `basic_ios` object. `rdbuf` is not destroyed.

### Notes

This destructor is `virtual` because the base class destructor,
`ios_base::~ios_base` is virtual.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 53 | C++98 | it was unspecified whether `rdbuf` is destroyed | it is not
      destroyed

---
*Source: https://en.cppreference.com/w/cpp/io/basic_ios/%7Ebasic_ios*
