# std::exception::what

```cpp
virtual const char* what() const throw();  // (until C++11)
virtual const char* what() const noexcept;  // (since C++11)
```

Returns the explanatory string.

### Parameters

(none)

### Return value

Pointer to a null-terminated string with explanatory information. The pointer is
guaranteed to be valid at least until the exception object from which it is
obtained is destroyed, or until a non-const member function on the exception
object is called.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 108 | C++98 | it was unspecified when the returned pointer becomes invalid
      | specified

---
*Source: https://en.cppreference.com/w/cpp/error/exception/what*
