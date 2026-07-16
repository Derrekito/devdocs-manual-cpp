# std::exception::exception

```cpp
exception() throw();  // (until C++11)
exception() noexcept;  // (since C++11)
exception( const exception& other ) throw();  // (until C++11)
exception( const exception& other ) noexcept;  // (since C++11)
```

Constructs new exception object.

1) Default constructor. `what()` returns an implementation-defined string.

2) Copy constructor. Initializes the contents with those of `other`. If `*this`
   and `other` both have dynamic type `std::exception` then `std::strcmp(what(),
   other.what()) == 0`.

### Parameters

- **other** — another exception to assign the contents of

### Notes

Because copying `std::exception` is not permitted to throw exceptions, when
derived classes (such as `std::runtime_error`) have to manage a user-defined
diagnostic message, it is typically implemented as a copy-on-write string.

The Microsoft implementation includes non-standard constructors taking strings
thus allowing instances to be thrown directly with a meaningful error message.
The nearest standard equivalents are `std::runtime_error` or `std::logic_error`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 471 | C++98 | there is no requirement on `what()` of the exception copy |
      added

---
*Source: https://en.cppreference.com/w/cpp/error/exception/exception*
