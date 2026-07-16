# std::chrono::weekday

```cpp
class weekday;  // (since C++20)
inline constexpr std::chrono::weekday Sunday{0};  // (since C++20)
inline constexpr std::chrono::weekday Monday{1};  // (since C++20)
inline constexpr std::chrono::weekday Tuesday{2};  // (since C++20)
inline constexpr std::chrono::weekday Wednesday{3};  // (since C++20)
inline constexpr std::chrono::weekday Thursday{4};  // (since C++20)
inline constexpr std::chrono::weekday Friday{5};  // (since C++20)
inline constexpr std::chrono::weekday Saturday{6};  // (since C++20)
```

The class `weekday` represent a day of the week in the proleptic Gregorian
calendar. Its normal range is `[`​0​`,`6`]`, for Sunday through Saturday, but it
can hold any value in the range `[`​0​`,`255`]`. Seven named constants are
predefined in the `std::chrono` namespace for the seven days of the week.

`weekday` is a TriviallyCopyable StandardLayoutType.

### Member functions

- **(constructor)** — constructs a `weekday` (public member function)
- **operator++operator++(int)operator--operator--(int)** — increments or
  decrements the weekday (public member function)
- **operator+=operator-=** — adds or subtracts a number of days (public member
  function)
- **c_encodingiso_encoding** — retrieves the stored weekday value retrieves ISO
  8601 weekday value (public member function)
- **ok** — checks if the stored weekday value is valid (public member function)
- **operator[]** — convenience syntax for constructing a `weekday_indexed` or
  `weekday_last` from this `weekday` (public member function)

### Nonmember functions

- **operator== (C++20)** — compares two `weekday` values (function)
- **operator+operator- (C++20)** — performs arithmetic on `weekday`s (function)
- **operator<< (C++20)** — outputs a `weekday` into a stream (function template)
- **from_stream (C++20)** — parses a `weekday` from a stream according to the
  provided format (function template)

### Helper classes

- **std::formatter<std::chrono::weekday> (C++20)** — formatting support for
  `weekday` (class template specialization)
- **std::hash<std::chrono::weekday> (C++26)** — hash support for
  **`std::chrono::weekday`** (class template specialization)

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    std::chrono::weekday x{42 / 13};
    std::cout << x++ << '\n';
    std::cout << x << '\n';
    std::cout << ++x << '\n';
}
```

Output:

```text
Wed
Thu
Fri
```

### See also

- **weekday_indexed (C++20)** — represents the nth **`weekday`** of a month
  (class)

---
*Source: https://en.cppreference.com/w/cpp/chrono/weekday*
