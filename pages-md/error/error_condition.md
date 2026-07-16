# std::error_condition

```cpp
class error_condition;  // (since C++11)
```

`std::error_condition` holds a platform-independent value identifying an error
condition. Like `std::error_code`, it is uniquely identified by an integer value
and a `std::error_category`, but unlike `std::error_code`, the value is not
platform-dependent.

A typical implementation holds one integer data member (the value) and a pointer
to an `std::error_category`.

### Member functions

- **(constructor)** — constructs an `error_condition` (public member function)
- **operator=** — replaces the contents (public member function)
- **assign** — replaces the contents (public member function)
- **clear** — sets the `error_condition` to value ​0​ in `generic_category`
  (public member function)
- **value** — obtains the value of the `error_condition` (public member
  function)
- **category** — obtains the `error_category` for this `error_condition` (public
  member function)
- **message** — obtains the explanatory string (public member function)
- **operator bool** — checks if the value is non-zero (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=> (removed in C++20)(removed in
  C++20)(C++20)** — compares `error_condition`s and `error_code`s (function)

### Helper classes

- **is_error_condition_enum (C++11)** — identifies an enumeration as an
  `std::error_condition` (class template)
- **std::hash<std::error_condition> (C++17)** — hash support for
  **`std::error_condition`** (class template specialization)

### See also

- **error_code (C++11)** — holds a platform-dependent error code (class)
- **error_category (C++11)** — base class for error categories (class)
- **make_error_condition(std::errc) (C++11)** — creates an error condition for
  an `errc` value `e` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition*
