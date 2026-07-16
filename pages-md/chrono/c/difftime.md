# std::difftime

```cpp
double difftime( std::time_t time_end, std::time_t time_beg );
```

Computes difference between two calendar times as `std::time_t` objects
(`time_end - time_beg`) in seconds. If `time_end` refers to time point before
`time_beg` then the result is negative.

### Parameters

- **time_beg, time_end** — times to compare

### Return value

Difference between two times in seconds.

### Notes

On POSIX systems, `std::time_t` is measured in seconds, and `difftime` is
equivalent to arithmetic subtraction, but C and C++ allow fractional units for
`time_t`.

### Example

```cpp
#include <ctime>
#include <iostream>

int main()
{
    std::time_t start = std::time(nullptr);
    volatile double d = 1.0;

    // some time-consuming operation
    for (int p = 0; p < 10000; ++p)
        for (int q = 0; q < 100000; ++q)
            d = d + p * d * q + d;

    std::cout << "Wall time passed: "
              << std::difftime(std::time(nullptr), start) << " s.\n";
}
```

Possible output:

```text
Wall time passed: 9 s.
```

### See also

- **duration (C++11)** — a time interval (class template)

**C documentation for `difftime`**

---
*Source: https://en.cppreference.com/w/cpp/chrono/c/difftime*
