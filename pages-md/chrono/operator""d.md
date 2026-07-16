# std::literals::chrono_literals::operator""d

```cpp
constexpr std::chrono::day operator ""d( unsigned long long d ) noexcept;  // (since C++20)
```

Forms a `std::chrono::day` literal representing a day of the month in the
calendar.

### Parameters

- **d** — the day value

### Return value

A `std::chrono::day` storing `d`. If `d > 255`, the stored value is unspecified.

### Possible implementation

```cpp
constexpr std::chrono::day operator ""d(unsigned long long d) noexcept
{
    return std::chrono::day(d);
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
    using namespace std::literals::chrono_literals;

    std::cout << static_cast<unsigned>(42d) << '\t' << 42d << '\n'
              << static_cast<unsigned>(256d) << '\t' << 256d << '\n' // unspecified
              << static_cast<unsigned>(298d) << '\t' << 298d << '\n'; // unspecified
}
```

Possible output:

```text
42      42 is not a valid day
0       00 is not a valid day
42      42 is not a valid day
```

### See also

- **(constructor)** — constructs a `day` (public member function of
  `std::chrono::day`)
- **operator unsigned** — retrieves the stored day value (public member function
  of `std::chrono::day`)

---
*Source: https://en.cppreference.com/w/cpp/chrono/operator%22%22d*
