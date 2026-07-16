# std::chrono::operator==,<,<=,>,>=,<=>(std::chrono::leap_second)

```cpp
constexpr bool operator==( const std::chrono::leap_second& x,
                           const std::chrono::leap_second& y ) noexcept;  // (1) (since C++20)
constexpr std::strong_ordering operator<=>( const std::chrono::leap_second& x,
                                            const std::chrono::leap_second& y ) noexcept;  // (2) (since C++20)
template< class Duration >
constexpr bool operator==( const std::chrono::leap_second& x,
                           const std::chrono::sys_time<Duration>& y ) noexcept;  // (3) (since C++20)
template< class Duration >
constexpr bool operator< ( const std::chrono::leap_second& x,
                           const std::chrono::sys_time<Duration>& y ) noexcept;  // (4) (since C++20)
template< class Duration >
constexpr bool operator< ( const std::chrono::sys_time<Duration>& x,
                           const std::chrono::leap_second& y ) noexcept;  // (5) (since C++20)
template< class Duration >
constexpr bool operator> ( const std::chrono::leap_second& x,
                           const std::chrono::sys_time<Duration>& y ) noexcept;  // (6) (since C++20)
template< class Duration >
constexpr bool operator> ( const std::chrono::sys_time<Duration>& x,
                           const std::chrono::leap_second& y ) noexcept;  // (7) (since C++20)
template< class Duration >
constexpr bool operator<=( const std::chrono::leap_second& x,
                           const std::chrono::sys_time<Duration>& y ) noexcept;  // (8) (since C++20)
template< class Duration >
constexpr bool operator<=( const std::chrono::sys_time<Duration>& x,
                           const std::chrono::leap_second& y ) noexcept;  // (9) (since C++20)
template< class Duration >
constexpr bool operator>=( const std::chrono::leap_second& x,
                           const std::chrono::sys_time<Duration>& y ) noexcept;  // (10) (since C++20)
template< class Duration >
constexpr bool operator>=( const std::chrono::sys_time<Duration>& x,
                           const std::chrono::leap_second& y ) noexcept;  // (11) (since C++20)
template< class Duration >
    requires std::three_way_comparable_with<
        std::chrono::sys_seconds, std::chrono::sys_time<Duration>>
constexpr auto operator<=>( const std::chrono::leap_second& x,
                            const std::chrono::sys_time<Duration>& y ) noexcept;  // (12) (since C++20)
```

Compares the date and time represented by the objects `x` and `y`.

Return type of (12) is deduced from `x.date() <=> y`, and hence the three-way
comparison result type of `std::chrono::seconds` and `Duration`.

The `!=` operator is synthesized from `operator==`.

### Return value

1) `x.date() == y.date()`

2) `x.date() <=> y.date()`

3) `x.date() == y`

4) `x.date() < y`

5) `x < y.date()`

6) `x.date() > y`

7) `x > y.date()`

8) `x.date() <= y`

9) `x <= y.date()`

10) `x.date() >= y`

11) `x >= y.date()`

12) `x.date() <=> y`

---
*Source: https://en.cppreference.com/w/cpp/chrono/leap_second/operator_cmp*
