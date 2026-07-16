# std::chrono::duration<Rep,Period>::zero

```cpp
static constexpr duration zero();  // (until C++20)
static constexpr duration zero() noexcept;  // (since C++20)
```

Returns a zero-length duration.

If the representation `rep` of the duration requires some other implementation
to return a zero-length duration, `std::chrono::duration_values` can be
specialized to return the desired value.

### Parameters

(none)

### Return value

Returns `duration(std::chrono::duration_values<rep>::zero())`.

### Example

```cpp
#include <chrono>
#include <ratio>

template<typename X, typename Y>
using dura = std::chrono::duration<X, Y>;

static_assert
(
    (std::chrono::hours::zero() == std::chrono::nanoseconds::zero()) &&
    (dura<short, std::nano>::zero() == dura<int, std::deci>::zero()) &&
    (dura<short, std::deca>::zero() == dura<long, std::exa>::zero()) &&
    (dura<long, std::atto>::zero().count() == dura<float, std::exa>::zero().count())
);

int main() {}
```

### See also

- **min [static]** — returns the special duration value min (public static
  member function)
- **max [static]** — returns the special duration value max (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/zero*
