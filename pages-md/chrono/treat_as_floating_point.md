# std::chrono::treat_as_floating_point

```cpp
template< class Rep >
struct treat_as_floating_point : std::is_floating_point<Rep> {};  // (since C++11)
```

The `std::chrono::treat_as_floating_point` trait helps determine if a duration
can be converted to another duration with a different tick period.

Implicit conversions between two durations normally depends on the tick period
of the durations. However, implicit conversions can happen regardless of tick
period if `std::chrono::treat_as_floating_point<Rep>::value == true`.

### Helper variable template

```cpp
template< class Rep >
inline constexpr bool treat_as_floating_point_v = treat_as_floating_point<Rep>::value;  // (since C++17)
```

### Specializations

`std::chrono::treat_as_floating_point` may be specialized for program-defined
types.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

void timed_piece_of_code()
{
    std::chrono::milliseconds simulated_work(2);
    std::this_thread::sleep_for(simulated_work);
}

int main()
{
    auto start = std::chrono::high_resolution_clock::now();

    std::cout << "Running some timed piece of code...\n";
    timed_piece_of_code();

    auto stop = std::chrono::high_resolution_clock::now();

    // A floating point milliseconds type
    using FpMilliseconds =
        std::chrono::duration<float, std::chrono::milliseconds::period>;

    static_assert(std::chrono::treat_as_floating_point<FpMilliseconds::rep>::value,
                  "Rep required to be floating point");

    // Note that implicit conversion is not allowed here
    auto i_ms = std::chrono::duration_cast<std::chrono::milliseconds>(stop - start);

    // Note that implicit conversion is allowed here
    auto f_ms = FpMilliseconds(stop - start);

    std::cout << "Timing stats:\n";

    std::cout << "  Time in milliseconds, using default rep: "
              << i_ms.count() << '\n';

    std::cout << "  Time in milliseconds, using floating point rep: "
              << f_ms.count() << '\n';
}
```

Possible output:

```text
Running some timed piece of code...
Timing stats:
  Time in milliseconds, using default rep: 2
  Time in milliseconds, using floating point rep: 2.57307
```

### See also

---
*Source: https://en.cppreference.com/w/cpp/chrono/treat_as_floating_point*
