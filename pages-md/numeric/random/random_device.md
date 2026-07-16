# std::random_device

```cpp
class random_device;  // (since C++11)
```

`std::random_device` is a uniformly-distributed integer random number generator
that produces non-deterministic random numbers.

`std::random_device` may be implemented in terms of an implementation-defined
pseudo-random number engine if a non-deterministic source (e.g. a hardware
device) is not available to the implementation. In this case each
`std::random_device` object may generate the same number sequence.

### Member types

- **`result_type` (C++11)** — unsigned int

### Member functions

**Construction**

- **(constructor) (C++11)** — constructs the engine (public member function)
- **operator= (deleted) (C++11)** — the assignment operator is deleted (public
  member function)

**Generation**

- **operator() (C++11)** — advances the engine's state and returns the generated
  value (public member function)

**Characteristics**

- **entropy (C++11)** — obtains the entropy estimate for the non-deterministic
  random number generator (public member function)
- **min [static] (C++11)** — gets the smallest possible value in the output
  range (public static member function)
- **max [static] (C++11)** — gets the largest possible value in the output range
  (public static member function)

### Notes

A notable implementation where `std::random_device` is deterministic in old
versions of MinGW-w64 (bug 338, fixed since GCC 9.2). The latest MinGW-w64
versions can be downloaded from GCC with the MCF thread model.

### Example

```cpp
#include <iostream>
#include <map>
#include <random>
#include <string>

int main()
{
    std::random_device rd;
    std::map<int, int> hist;
    std::uniform_int_distribution<int> dist(0, 9);

    for (int n = 0; n != 20000; ++n)
        ++hist[dist(rd)]; // note: demo only: the performance of many
                          // implementations of random_device degrades sharply
                          // once the entropy pool is exhausted. For practical use
                          // random_device is generally only used to seed
                          // a PRNG such as mt19937

    for (auto [x, y] : hist)
        std::cout << x << " : " << std::string(y / 100, '*') << '\n';
}
```

Possible output:

```text
0 : ********************
1 : *******************
2 : ********************
3 : ********************
4 : ********************
5 : *******************
6 : ********************
7 : ********************
8 : *******************
9 : ********************
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/random_device*
