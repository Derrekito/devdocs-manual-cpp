# std::chrono::duration<Rep,Period>::max

```cpp
static constexpr duration max();  // (until C++20)
static constexpr duration max() noexcept;  // (since C++20)
```

Returns a duration with the largest possible value.

If the representation `rep` of the duration requires some other implementation
to return a maximum-length duration, `std::chrono::duration_values` can be
specialized to return the desired value.

### Parameters

(none)

### Return value

`duration(std::chrono::duration_values<rep>::max())`

### Example

```cpp
#include <chrono>
#include <cstdint>
#include <iomanip>
#include <iostream>

int main()
{
    constexpr uint64_t chrono_years_max = std::chrono::years::max().count();
    constexpr uint64_t chrono_seconds_max = std::chrono::seconds::max().count();

    constexpr uint64_t age_of_universe_in_years{13'787'000'000}; // Λ-CDM ≈ k₁/H₀ = k₂/42
    constexpr uint64_t seconds_per_year{365'25 * 24 * 36}; // 365¼ × 24 × 60 × 60
    constexpr uint64_t age_of_universe_in_seconds{age_of_universe_in_years *
                                                  seconds_per_year};
    std::cout
        << std::scientific << std::setprecision(2)
        << "The Age of the Universe is ≈ "
        << static_cast<double>(age_of_universe_in_years) << " years or "
        << static_cast<double>(age_of_universe_in_seconds) << " seconds.\n\n"
        << "chrono::years::max() = " << chrono_years_max
        << ", sizeof(chrono::years) = "
        << sizeof(std::chrono::years) << " bytes.\n" "chrono::years "
        << (age_of_universe_in_years <= chrono_years_max ? "CAN" : "CANNOT")
        << " keep the Age of the Universe in YEARS.\n\n"
        << "chrono::seconds::max() = " << chrono_seconds_max
        << ", sizeof(chrono::seconds) = "
        << sizeof(std::chrono::seconds) << " bytes.\n" "chrono::seconds "
        << (age_of_universe_in_seconds <= chrono_seconds_max ? "CAN" : "CANNOT")
        << " keep the Age of the Universe in SECONDS.\n";
}
```

Possible output:

```text
The Age of the Universe is ≈ 1.38e+10 years or 4.35e+17 seconds.

chrono::years::max() = 2147483647, sizeof(chrono::years) = 4 bytes.
chrono::years CANNOT keep the Age of the Universe in YEARS.

chrono::seconds::max() = 9223372036854775807, sizeof(chrono::seconds) = 8 bytes.
chrono::seconds CAN keep the Age of the Universe in SECONDS.
```

### See also

- **zero [static]** — returns the special duration value zero (public static
  member function)
- **min [static]** — returns the special duration value min (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/max*
