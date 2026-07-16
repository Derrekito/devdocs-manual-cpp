# C++ named requirements: Clock (since C++11)

The **Clock** requirements describe a bundle consisting of a
`std::chrono::duration`, a `std::chrono::time_point`, and a function `now()` to
get the current `time_point`. The origin of the clock's `time_point` is referred
to as the clock's epoch.

### Requirements

`C1` and `C2` denote clock types. `t1` and `t2` are values returned by
`C1::now()` where the call returning `t1` happens before the call returning `t2`
and both of these calls occur before `C1::time_point::max()`.

  Expression | Return type | Operational semantics
  `C1::rep` | An arithmetic type or a class emulating an arithmetic type | The
      representation type of `C1::duration`.
  `C1::period` | A specialization of `std::ratio` | The tick period of the clock
      in seconds.
  `C1::duration` | `std::chrono::duration<C1::rep, C1::period>` | The duration
      type of the clock.
  `C1::time_point` | `std::chrono::time_point<C1>` or
      `std::chrono::time_point<C2, C1::duration>` | The
      `std::chrono::time_point` type of the clock. C1 and C2 shall refer to the
      same epoch.
  `C1::is_steady` | `const bool` | `true` if `t1 <= t2` is always `true` and the
      clock advances at a steady rate relative to real time (so the difference
      between two distinct reported times is approximately the elapsed real time
      between the clock updates), otherwise `false`
  `C1::now()` | `C1::time_point` | Returns a `time_point` object representing
      the current point in time.

### See also

- **is_clockis_clock_v (C++20)** — determines if a type is a **Clock** (class
  template) (variable template)

---
*Source: https://en.cppreference.com/w/cpp/named_req/Clock*
