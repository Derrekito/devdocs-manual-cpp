# std::chrono::leap_second

```cpp
class leap_second;  // (since C++20)
```

The class `leap_second` represents the date and time of a leap second insertion.

Users cannot construct `leap_second` objects except by copying from existing
`leap_second` objects. The library implementation creates `leap_second` objects
when it initializes the time zone database and provides const access to these
objects.

`leap_second` has a defaulted copy constructor and a defaulted copy assignment
operator.

### Member functions

- **date** — obtains the time of leap second insertion (public member function)

### Nonmember functions

- **operator==operator<operator<=operator>operator>=operator<=> (C++20)** —
  compares two `leap_second` values or a `leap_second` value and a `sys_time`
  (function template)

### Helper classes

- **std::hash<std::chrono::leap_second> (C++26)** — hash support for
  **`std::chrono::leap_second`** (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/chrono/leap_second*
