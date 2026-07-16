# std::chrono::time_zone

```cpp
class time_zone;  // (since C++20)
```

The class `time_zone` represents all time zone transitions for a specific
geographic area.

Users cannot construct `time_zone` objects. The library implementation creates
`time_zone` objects when it initializes the time zone database and provides
`const` access to these objects.

`time_zone` is not copyable but does have a defaulted move constructor and a
defaulted move assignment operator. However, as users have only `const` access
to `time_zone` objects, these functions cannot be called in user code without
invoking undefined behavior.

### Member functions

- **name** — obtains the name of this `time_zone` (public member function)
- **get_info** — obtain information associated with a `sys_time` or `local_time`
  (public member function)
- **to_sys** — converts a `local_time` in this time zone to a `sys_time` (public
  member function)
- **to_local** — converts a `sys_time` to a `local_time` in this time zone
  (public member function)

### Nonmember functions

- **operator==operator<=> (C++20)** — compares two `time_zone` objects
  (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_zone*
