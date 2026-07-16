# Standard library header <stdexcept>

This header is part of the error handling library.

**Classes**

- **logic_error** — exception class to indicate violations of logical
  preconditions or class invariants (class)
- **invalid_argument** — exception class to report invalid arguments (class)
- **domain_error** — exception class to report domain errors (class)
- **length_error** — exception class to report attempts to exceed maximum
  allowed size (class)
- **out_of_range** — exception class to report arguments outside of expected
  range (class)
- **runtime_error** — exception class to indicate conditions only detectable at
  run time (class)
- **range_error** — exception class to report range errors in internal
  computations (class)
- **overflow_error** — exception class to report arithmetic overflows (class)
- **underflow_error** — exception class to report arithmetic underflows (class)

### Synopsis

```cpp
namespace std {
    class logic_error;
    class domain_error;
    class invalid_argument;
    class length_error;
    class out_of_range;
    class runtime_error;
    class range_error;
    class overflow_error;
    class underflow_error;
}
```

#### Class `std::logic_error`

```cpp
namespace std {
  class logic_error : public exception {
  public:
    explicit logic_error(const string& what_arg);
    explicit logic_error(const char* what_arg);
  };
}
```

#### Class `std::domain_error`

```cpp
namespace std {
  class domain_error : public logic_error {
  public:
    explicit domain_error(const string& what_arg);
    explicit domain_error(const char* what_arg);
  };
}
```

#### Class `std::invalid_argument`

```cpp
namespace std {
  class invalid_argument : public logic_error {
  public:
    explicit invalid_argument(const string& what_arg);
    explicit invalid_argument(const char* what_arg);
  };
}
```

#### Class `std::length_error`

```cpp
namespace std {
  class length_error : public logic_error {
  public:
    explicit length_error(const string& what_arg);
    explicit length_error(const char* what_arg);
  };
}
```

#### Class `std::out_of_range`

```cpp
namespace std {
  class out_of_range : public logic_error {
  public:
    explicit out_of_range(const string& what_arg);
    explicit out_of_range(const char* what_arg);
  };
}
```

#### Class `std::runtime_error`

```cpp
namespace std {
  class runtime_error : public exception {
  public:
    explicit runtime_error(const string& what_arg);
    explicit runtime_error(const char* what_arg);
  };
}
```

#### Class `std::range_error`

```cpp
namespace std {
  class range_error : public runtime_error {
  public:
    explicit range_error(const string& what_arg);
    explicit range_error(const char* what_arg);
  };
}
```

#### Class `std::overflow_error`

```cpp
namespace std {
  class overflow_error : public runtime_error {
  public:
    explicit overflow_error(const string& what_arg);
    explicit overflow_error(const char* what_arg);
  };
}
```

#### Class `std::underflow_error`

```cpp
namespace std {
  class underflow_error : public runtime_error {
  public:
    explicit underflow_error(const string& what_arg);
    explicit underflow_error(const char* what_arg);
  };
}
```

### See also

- **exception** — base class for exceptions thrown by the standard library
  components (class)

---
*Source: https://en.cppreference.com/w/cpp/header/stdexcept*
