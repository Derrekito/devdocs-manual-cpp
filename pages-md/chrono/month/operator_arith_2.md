# std::chrono::operator+, std::chrono::operator- (std::chrono::month)

```cpp
constexpr std::chrono::month operator+( const std::chrono::month& m,
                                        const std::chrono::months& ms ) noexcept;  // (1) (since C++20)
constexpr std::chrono::month operator+( const std::chrono::months& ms,
                                        const std::chrono::month& m ) noexcept;  // (2) (since C++20)
constexpr std::chrono::month operator-( const std::chrono::month& m,
                                        const std::chrono::months& ms ) noexcept;  // (3) (since C++20)
constexpr std::chrono::months operator-( const std::chrono::month& m1,
                                         const std::chrono::month& m2 ) noexcept;  // (4) (since C++20)
```

1,2) Adds `ms.count()` months to `m`. The month value held in the result is
   computed by first evaluating `static_cast<long long>(unsigned(m)) +
   (ms.count() - 1)`, reducing it modulo 12 to an integer in the range
   `[``​0​``,``11``]`, and then adding 1.

3) Subtracts `ms.count()` months from `m` and returns the result. Equivalent to
   `return m + -ms;`.

4) If `m1.ok()` and `m2.ok()` are both `true`, returns a `std::chrono::months`
   value `m` such that `m.count()` is in the range `[``​0​``,``11``]` and `m2 +
   m == m1`. Otherwise the returned value is unspecified.

### Return value

1-3) A `std::chrono::month` holding a month value calculated as described above.

4) A `std::chrono::months` representing the distance between `m1` and `m2`.

### Notes

As long as the computation doesn't overflow, (1-3) always return a valid month
even if `m.ok()` is `false`.

The result of subtracting two `month` values is a duration of type
`std::chrono::months`. That duration unit represents the length of the average
Gregorian month, and the resulting duration bears no relationship to the number
of days in the particular months represented by the operands. For example,
`std::chrono::seconds(std::chrono::April - std::chrono::March)` is not the
number of seconds in March (`2678400s`), but `2629746s` (30.436875 days).

### Example

```cpp
#include <cassert>
#include <chrono>

int main()
{
    std::chrono::month m{6};

    m = m + std::chrono::months(2);
    assert(m == std::chrono::month(8));

    m = m - std::chrono::months(3);
    assert(m == std::chrono::month(5));

    constexpr std::chrono::months ms = std::chrono::month(8) - std::chrono::month(6);
    static_assert(ms == std::chrono::months(2));
}
```

### See also

- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the month (public member function)
- **operator+=operator-=** — adds or subtracts a number of months (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/operator_arith_2*
