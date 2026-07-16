# deduction guides for `std::reference_wrapper`

```cpp
template<typename T>
reference_wrapper(T&) -> reference_wrapper<T>;  // (since C++17)
```

One deduction guide is provided for `std::reference_wrapper` to support
deduction of the sole class template parameter.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2981 | C++17 | a redundant deduction guide from `reference_wrapper<T>` was
      provided | removed
  LWG 2993 | C++17 | defect resolution removed a constructor used for class
      template argument deduction | added deduction guide to compensate

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper/deduction_guides*
