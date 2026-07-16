# std::error_condition::message

```cpp
std::string message() const;  // (since C++11)
```

Returns an explanatory message for the stored error value and error category.
Effectively calls `category().message(value())`.

### Parameters

(none)

### Return value

An explanatory message for the stored error value and error category.

### Exceptions

May throw implementation-defined exceptions.

### See also

- **message [virtual]** — obtains the explanatory string (virtual public member
  function of `std::error_category`)

---
*Source: https://en.cppreference.com/w/cpp/error/error_condition/message*
