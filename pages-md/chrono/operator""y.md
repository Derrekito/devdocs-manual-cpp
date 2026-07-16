# std::literals::chrono_literals::operator""y

```cpp
constexpr std::chrono::year operator""y( unsigned long long y ) noexcept;  // (since C++20)
```

Forms a `std::chrono::year` literal representing a year in the proleptic
Gregorian calendar.

### Parameters

- **y** — the year value

### Return value

A `std::chrono::year` initialized from `int(y)`. If `y` is not in the range
`[``-32767``,``32767``]`, the stored value is unspecified.

### Possible implementation

```cpp
constexpr std::chrono::year operator""y(unsigned long long y) noexcept
{
    return std::chrono::year(static_cast<int>(y));
}
```

### Notes

This operator is declared in the namespace `std::literals::chrono_literals`,
where both `literals` and `chrono_literals` are inline namespaces. Access to
this operator can be gained with:

- `using namespace std::literals`,
- `using namespace std::chrono_literals`, or
- `using namespace std::literals::chrono_literals`.

In addition, within the namespace `std::chrono`, the directive `using namespace
literals::chrono_literals;` is provided by the standard library, so that if a
programmer uses `using namespace std::chrono;` to gain access to the classes in
the chrono library, the corresponding literal operators become visible as well.

### Example

```cpp
#include <chrono>
#include <iostream>

int main()
{
    using namespace std::literals;

    std::cout << int(2020y)  << '\t' << 2020y  << '\n'
              << int(-220y)  << '\t' << -220y  << '\n'
              << int(3000y)  << '\t' << 3000y  << '\n'
              << int(32768y) << '\t' << 32768y << '\n'  // unspecified
              << int(65578y) << '\t' << 65578y << '\n'; // unspecified
}
```

Possible output:

```text
2020        2020
-220        -0220
3000        3000
-32768        -32768 is not a valid year
42        0042
```

### See also

- **(constructor)** — constructs a `year` (public member function of
  `std::chrono::year`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/operator%22%22y*
