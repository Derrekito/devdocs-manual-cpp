# Standard library header <exception>

This header is part of the error handling library.

**Types**

- **exception** — base class for exceptions thrown by the standard library
  components (class)
- **nested_exception (C++11)** — a mixin type to capture and store current
  exceptions (class)
- **bad_exception** — exception thrown when `std::current_exception` fails to
  copy the exception object (class)
- **unexpected_handler (removed in C++17)** — the type of the function called by
  `std::unexpected` (typedef)
- **terminate_handler** — the type of the function called by `std::terminate`
  (typedef)
- **exception_ptr (C++11)** — shared pointer type for handling exception objects
  (typedef)

**Functions**

- **unexpected (removed in C++17)** — function called when dynamic exception
  specification is violated (function)
- **uncaught_exceptionuncaught_exceptions (removed in C++20)(C++17)** — checks
  if exception handling is currently in progress (function)
- **make_exception_ptr (C++11)** — creates an `std::exception_ptr` from an
  exception object (function template)
- **current_exception (C++11)** — captures the current exception in a
  `std::exception_ptr` (function)
- **rethrow_exception (C++11)** — throws the exception from an
  `std::exception_ptr` (function)
- **throw_with_nested (C++11)** — throws its argument with
  `std::nested_exception` mixed in (function template)
- **rethrow_if_nested (C++11)** — throws the exception from a
  `std::nested_exception` (function template)
- **terminate** — function called when exception handling fails (function)
- **get_terminate (C++11)** — obtains the current terminate_handler (function)
- **set_terminate** — changes the function to be called by `std::terminate`
  (function)
- **get_unexpected (C++11)(removed in C++17)** — obtains the current
  unexpected_handler (function)
- **set_unexpected (removed in C++17)** — changes the function to be called by
  `std::unexpected` (function)

### Synopsis

```cpp
namespace std {
  class exception;
  class bad_exception;
  class nested_exception;

  using terminate_handler = void (*)();
  terminate_handler get_terminate() noexcept;
  terminate_handler set_terminate(terminate_handler f) noexcept;
  [[noreturn]] void terminate() noexcept;

  int uncaught_exceptions() noexcept;

  using exception_ptr = /* unspecified */;

  exception_ptr current_exception() noexcept;
  [[noreturn]] void rethrow_exception(exception_ptr p);
  template<class E> exception_ptr make_exception_ptr(E e) noexcept;

  template<class T> [[noreturn]] void throw_with_nested(T&& t);
  template<class E> void rethrow_if_nested(const E& e);
}
```

#### Class `std::exception`

```cpp
namespace std {
  class exception {
  public:
    exception() noexcept;
    exception(const exception&) noexcept;
    exception& operator=(const exception&) noexcept;
    virtual ~exception();
    virtual const char* what() const noexcept;
  };
}
```

#### Class `std::bad_exception`

```cpp
namespace std {
  class bad_exception : public exception {
  public:
    // see [exception] for the specification of the special member functions
    const char* what() const noexcept override;
  };
}
```

#### Class `std::nested_exception`

```cpp
namespace std {
  class nested_exception {
  public:
    nested_exception() noexcept;
    nested_exception(const nested_exception&) noexcept = default;
    nested_exception& operator=(const nested_exception&) noexcept = default;
    virtual ~nested_exception() = default;

    // access functions
    [[noreturn]] void rethrow_nested() const;
    exception_ptr nested_ptr() const noexcept;
  };

  template<class T> [[noreturn]] void throw_with_nested(T&& t);
  template<class E> void rethrow_if_nested(const E& e);
}
```

### See also

- Error handling

---
*Source: https://en.cppreference.com/w/cpp/header/exception*
