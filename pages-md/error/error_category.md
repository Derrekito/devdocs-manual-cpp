# std::error_category

```cpp
class error_category;  // (since C++11)
```

`std::error_category` serves as the base class for specific error category
types, such as `std::system_category`, `std::iostream_category`, etc. Each
specific category class defines the `error_code` - `error_condition` mapping and
holds the explanatory strings for all error_conditions. The objects of error
category classes are treated as singletons, passed by reference.

### Member functions

- **(constructor)** — constructs an `error_category` (public member function)
- **(destructor) [virtual]** — destructs an `error_category` (virtual public
  member function)
- **operator= [deleted]** — not copy assignable (public member function)
- **name [virtual]** — obtains the name of the category (virtual public member
  function)
- **default_error_condition [virtual]** — maps `error_code` to `error_condition`
  (virtual public member function)
- **equivalent [virtual]** — compares `error_code` and `error_condition` for
  equivalence (virtual public member function)
- **message [virtual]** — obtains the explanatory string (virtual public member
  function)
- **operator==operator!=operator<operator<=> (removed in C++20)(removed in
  C++20)(C++20)** — compares two error categories (function)

### Specific error categories

- **generic_category (C++11)** — identifies the generic error category
  (function)
- **system_category (C++11)** — identifies the operating system error category
  (function)
- **iostream_category (C++11)** — identifies the iostream error category
  (function)
- **future_category (C++11)** — identifies the future error category (function)
- **error_condition (C++11)** — holds a portable error code (class)
- **error_code (C++11)** — holds a platform-dependent error code (class)

---
*Source: https://en.cppreference.com/w/cpp/error/error_category*
