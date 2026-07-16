# std::chrono::weekday::operator++, std::chrono::weekday::operator--

```cpp
constexpr std::chrono::weekday& operator++() noexcept;  // (1) (since C++20)
constexpr std::chrono::weekday operator++( int ) noexcept;  // (2) (since C++20)
constexpr std::chrono::weekday& operator--() noexcept;  // (3) (since C++20)
constexpr std::chrono::weekday operator--( int ) noexcept;  // (4) (since C++20)
```

Adds or subtracts `1` from the weekday value, reducing the result modulo 7 to an
integer in the range `[``​0​``,``6``]`.

1,2) Performs `*this += std::chrono::days{1};`.

3,4) Performs `*this -= std::chrono::days{1};`.

### Parameters

(none)

### Return value

1,3) A reference to this `weekday` after modification.

2,4) A copy of the `weekday` made before modification.

### Notes

After a call to one of these functions, `ok()` is always `true`.

### Example

```cpp
#include <cassert>
#include <chrono>
#include <iostream>

int main()
{
    std::cout << std::boolalpha;

    std::chrono::weekday wd{0}; // Sunday is 0 or 7

    --wd;
    std::cout << (wd == std::chrono::Saturday) << ' ';

    ++wd;
    std::cout << (wd == std::chrono::Sunday) << '\n';

    wd = std::chrono::weekday{13};
    assert(!wd.ok());
    wd++;
    assert(wd.ok());
}
```

Output:

```text
true true
```

### See also

- **operator+=operator-=** — adds or subtracts a number of days (public member
  function)
- **operator+operator- (C++20)** — performs arithmetic on `weekday`s (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday/operator_inc_dec*
