# std::time_t

```cpp
typedef /* unspecified */ time_t;
```

Arithmetic type capable of representing times.

Although not defined, this is almost always an integral value holding the number
of seconds (not counting leap seconds) since 00:00, Jan 1 1970 UTC,
corresponding to POSIX time.

### See also

- **time** — returns the current time of the system as time since epoch
  (function)
- **localtime** — converts time since epoch to calendar time expressed as local
  time (function)
- **gmtime** — converts time since epoch to calendar time expressed as Universal
  Coordinated Time (function)
- **to_time_t [static]** — converts a system clock time point to `std::time_t`
  (public static member function of `std::chrono::system_clock`)

**C documentation for `time_t`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/time_t*
