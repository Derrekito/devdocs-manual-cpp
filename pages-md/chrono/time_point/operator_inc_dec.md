# std::chrono::time_point<Clock,Duration>::operator++, std::chrono::time_point<Clock,Duration>::operator--

```cpp
constexpr time_point& operator++();  // (1) (since C++20)
constexpr time_point operator++( int );  // (2) (since C++20)
constexpr time_point& operator--();  // (3) (since C++20)
constexpr time_point operator--( int );  // (4) (since C++20)
```

Modifies the point in time `*this` represents by one tick of the `duration`.

If `d_` is a member variable holding the duration (i.e., time since epoch) of
this `time_point` object,

1) Equivalent to `++d_; return *this;`.

2) Equivalent to `return time_point(d_++)`.

3) Equivalent to `--d_; return *this;`.

4) Equivalent to `return time_point(d_--);`.

### Parameters

(none)

### Return value

1,3) A reference to this `time_point` after modification.

2,4) A copy of the `time_point` made before modification.

### Example

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the tick count (public member function of
  `std::chrono::duration<Rep,Period>`)
- **operator+=operator-=** — modifies the time point by the given duration
  (public member function)
- **operator+operator- (C++11)** — performs add and subtract operations
  involving a time point (function template)

---
*Source: https://en.cppreference.com/w/cpp/chrono/time_point/operator_inc_dec*
