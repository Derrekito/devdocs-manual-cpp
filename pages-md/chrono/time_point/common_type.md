# std::common_type<std::chrono::time_point>

```cpp
template< class Clock, class Duration1, class Duration2 >
struct common_type<std::chrono::time_point<Clock, Duration1>,
                   std::chrono::time_point<Clock, Duration2>> {
    typedef std::chrono::time_point<
        Clock, typename std::common_type<Duration1, Duration2>::type> type;
};  // (since C++11)
```

Exposes the type named `type`, which is the common type of two
`std::chrono::time_point`s.

### Notes

The common type of two `std::chrono::time_point` types is a
`std::chrono::time_point` with the same clock as the two types and the
`std::common_type` of their durations.

### Example

### See also

- **std::common_type<std::chrono::duration> (C++11)** — specializes the
  `std::common_type` trait (class template specialization)
- **common_type (C++11)** — determines the common type of a group of types
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/common_type*
