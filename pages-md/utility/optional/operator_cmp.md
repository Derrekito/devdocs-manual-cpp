# operator==, !=, <, <=, >, >=, <=>(std::optional)

```cpp
Compare two optional objects
template< class T, class U >
constexpr bool operator==( const optional<T>& lhs, const optional<U>& rhs );  // (1) (since C++17)
template< class T, class U >
constexpr bool operator!=( const optional<T>& lhs, const optional<U>& rhs );  // (2) (since C++17)
template< class T, class U >
constexpr bool operator<( const optional<T>& lhs, const optional<U>& rhs );  // (3) (since C++17)
template< class T, class U >
constexpr bool operator<=( const optional<T>& lhs, const optional<U>& rhs );  // (4) (since C++17)
template< class T, class U >
constexpr bool operator>( const optional<T>& lhs, const optional<U>& rhs );  // (5) (since C++17)
template< class T, class U >
constexpr bool operator>=( const optional<T>& lhs, const optional<U>& rhs );  // (6) (since C++17)
template< class T, std::three_way_comparable_with<T> U >
constexpr std::compare_three_way_result_t<T, U>
    operator<=>( const optional<T>& lhs, const optional<U>& rhs );  // (7) (since C++20)
Compare an optional object with a nullopt
template< class T >
constexpr bool operator==( const optional<T>& opt, std::nullopt_t ) noexcept;  // (8) (since C++17)
template< class T >
constexpr bool operator==( std::nullopt_t, const optional<T>& opt ) noexcept;  // (9) (since C++17) (until C++20)
template< class T >
constexpr bool operator!=( const optional<T>& opt, std::nullopt_t ) noexcept;  // (10) (since C++17) (until C++20)
template< class T >
constexpr bool operator!=( std::nullopt_t, const optional<T>& opt ) noexcept;  // (11) (since C++17) (until C++20)
template< class T >
constexpr bool operator<( const optional<T>& opt, std::nullopt_t ) noexcept;  // (12) (since C++17) (until C++20)
template< class T >
constexpr bool operator<( std::nullopt_t, const optional<T>& opt ) noexcept;  // (13) (since C++17) (until C++20)
template< class T >
constexpr bool operator<=( const optional<T>& opt, std::nullopt_t ) noexcept;  // (14) (since C++17) (until C++20)
template< class T >
constexpr bool operator<=( std::nullopt_t, const optional<T>& opt ) noexcept;  // (15) (since C++17) (until C++20)
template< class T >
constexpr bool operator>( const optional<T>& opt, std::nullopt_t ) noexcept;  // (16) (since C++17) (until C++20)
template< class T >
constexpr bool operator>( std::nullopt_t, const optional<T>& opt ) noexcept;  // (17) (since C++17) (until C++20)
template< class T >
constexpr bool operator>=( const optional<T>& opt, std::nullopt_t ) noexcept;  // (18) (since C++17) (until C++20)
template< class T >
constexpr bool operator>=( std::nullopt_t, const optional<T>& opt ) noexcept;  // (19) (since C++17) (until C++20)
template< class T >
constexpr std::strong_ordering
    operator<=>( const optional<T>& opt, std::nullopt_t ) noexcept;  // (20) (since C++20)
Compare an optional object with a value
template< class T, class U >
constexpr bool operator==( const optional<T>& opt, const U& value );  // (21) (since C++17)
template< class T, class U >
constexpr bool operator==( const T& value, const optional<U>& opt );  // (22) (since C++17)
template< class T, class U >
constexpr bool operator!=( const optional<T>& opt, const U& value );  // (23) (since C++17)
template< class T, class U >
constexpr bool operator!=( const T& value, const optional<U>& opt );  // (24) (since C++17)
template< class T, class U >
constexpr bool operator<( const optional<T>& opt, const U& value );  // (25) (since C++17)
template< class T, class U >
constexpr bool operator<( const T& value, const optional<U>& opt );  // (26) (since C++17)
template< class T, class U >
constexpr bool operator<=( const optional<T>& opt, const U& value );  // (27) (since C++17)
template< class T, class U >
constexpr bool operator<=( const T& value, const optional<U>& opt );  // (28) (since C++17)
template< class T, class U >
constexpr bool operator>( const optional<T>& opt, const U& value );  // (29) (since C++17)
template< class T, class U >
constexpr bool operator>( const T& value, const optional<U>& opt );  // (30) (since C++17)
template< class T, class U >
constexpr bool operator>=( const optional<T>& opt, const U& value );  // (31) (since C++17)
template< class T, class U >
constexpr bool operator>=( const T& value, const optional<U>& opt );  // (32) (since C++17)
template< class T, std::three_way_comparable_with<T> U >
constexpr std::compare_three_way_result_t<T, U>
    operator<=>( const optional<T>& opt, const U& value );  // (33) (since C++20)
```

Performs comparison operations on `optional` objects.

1-7) Compares two `optional` objects, `lhs` and `rhs`. The contained values are
   compared (using the corresponding operator of `T`) only if both `lhs` and
   `rhs` contain values. Otherwise,

- `lhs` is considered *equal to* `rhs` if, and only if, both `lhs` and `rhs` do
  not contain a value.
- `lhs` is considered *less than* `rhs` if, and only if, `rhs` contains a value
  and `lhs` does not.

8-20) Compares `opt` with a `nullopt`. Equivalent to (1-6) when comparing to an
   `optional` that does not contain a value. The `<`, `<=`, `>`, `>=`, and `!=`
   operators are synthesized from operator<=> and operator== respectively.
   (since C++20)

21-33) Compares `opt` with a `value`. The values are compared (using the
   corresponding operator of `T`) only if `opt` contains a value. Otherwise,
   `opt` is considered *less than* `value`. If the corresponding two-way
   comparison expression between `*opt` and `value` is not well-formed, or if
   its result is not convertible to `bool`, the program is ill-formed.

### Parameters

- **lhs, rhs, opt** — an `optional` object to compare
- **value** — value to compare to the contained value

### Return value

1) If `bool(lhs) != bool(rhs)`, returns `false`. Otherwise, if `bool(lhs) ==
   false` (and so `bool(rhs) == false` as well), returns `true`. Otherwise,
   returns `*lhs == *rhs`.

2) If `bool(lhs) != bool(rhs)`, returns `true`. Otherwise, if `bool(lhs) ==
   false` (and so `bool(rhs) == false` as well), returns `false`. Otherwise,
   returns `*lhs != *rhs`.

3) If `bool(rhs) == false` returns `false`. Otherwise, if `bool(lhs) == false`,
   returns `true`. Otherwise returns `*lhs < *rhs`.

4) If `bool(lhs) == false` returns `true`. Otherwise, if `bool(rhs) == false`,
   returns `false`. Otherwise returns `*lhs <= *rhs`.

5) If `bool(lhs) == false` returns `false`. Otherwise, if `bool(rhs) == false`,
   returns `true`. Otherwise returns `*lhs > *rhs`.

6) If `bool(rhs) == false` returns `true`. Otherwise, if `bool(lhs) == false`,
   returns `false`. Otherwise returns `*lhs >= *rhs`.

7) If `bool(lhs) && bool(rhs)` is `true` returns `*x <=> *y`. Otherwise, returns
   `bool(lhs) <=> bool(rhs)`.

8,9) Returns `!opt`.

10,11) Returns `bool(opt)`.

12) Returns `false`.

13) Returns `bool(opt)`.

14) Returns `!opt`.

15) Returns `true`.

16) Returns `bool(opt)`.

17) Returns `false`.

18) Returns `true`.

19) Returns `!opt`.

20) Returns `bool(opt) <=> false`.

21) Returns `bool(opt) ? *opt == value : false`.

22) Returns `bool(opt) ? value == *opt : false`.

23) Returns `bool(opt) ? *opt != value : true`.

24) Returns `bool(opt) ? value != *opt : true`.

25) Returns `bool(opt) ? *opt < value : true`.

26) Returns `bool(opt) ? value < *opt : false`.

27) Returns `bool(opt) ? *opt <= value : true`.

28) Returns `bool(opt) ? value <= *opt : false`.

29) Returns `bool(opt) ? *opt > value : false`.

30) Returns `bool(opt) ? value > *opt : true`.

31) Returns `bool(opt) ? *opt >= value : false`.

32) Returns `bool(opt) ? value >= *opt : true`.

33) Returns `bool(opt) ? *opt <=> value : std::strong_ordering::less`.

### Exceptions

1-7) May throw implementation-defined exceptions.

21-33) Throws when and what the comparison throws.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2945 | C++17 | order of template parameters inconsistent for
      compare-with-T cases | made consistent

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/operator_cmp*
