# std::error_code

```cpp
class error_code;  // (since C++11)
```

`std::error_code` represents a platform-dependent error code value. Each
`std::error_code` object holds an error code value originating from the
operating system or some low-level interface and a pointer to an object of type
`std::error_category`, which corresponds to the said interface. The error code
values are not required to be unique across different error categories.

### Member functions

- **(constructor)** — constructs an error code (public member function)
- **operator=** — assigns another error code (public member function)
- **assign** — assigns another error code (public member function)

**Modifiers**

- **clear** — sets the `error_code` to value ​0​ in `system_category` (public
  member function)

**Observers**

- **value** — obtains the value of the `error_code` (public member function)
- **category** — obtains the error_category for this error_code (public member
  function)
- **default_error_condition** — obtains the `error_condition` for this
  `error_code` (public member function)
- **message** — obtains the explanatory string for this `error_code` (public
  member function)
- **operator bool** — checks if the value is non-zero (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=> (removed in C++20)(removed in
  C++20)(C++20)** — compares two `error_code`s (function)
- **operator<<** — outputs the value and the category name to an output stream
  (function)

### Helper classes

- **is_error_code_enum (C++11)** — identifies a class as an `error_code`
  enumeration (class template)
- **std::hash<std::error_code> (C++11)** — hash support for
  **`std::error_code`** (class template specialization)

### See also

- **error_condition (C++11)** — holds a portable error code (class)
- **error_category (C++11)** — base class for error categories (class)
- **make_error_code(std::errc) (C++11)** — creates error code value for `errc`
  enum `e` (function)

---
*Source: https://en.cppreference.com/w/cpp/error/error_code*
