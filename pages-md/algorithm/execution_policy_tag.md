# std::execution::seq, std::execution::par, std::execution::par_unseq, std::execution::unseq

```cpp
inline constexpr
std::execution::sequenced_policy seq { /* unspecified */ };  // (since C++17)
inline constexpr
std::execution::parallel_policy par { /* unspecified */ };  // (since C++17)
inline constexpr
std::execution::parallel_unsequenced_policy par_unseq { /* unspecified */ };  // (since C++17)
inline constexpr
std::execution::unsequenced_policy unseq { /* unspecified */ };  // (since C++20)
```

The execution policy types

- `std::execution::sequenced_policy`,
- `std::execution::parallel_policy`,
- `std::execution::parallel_unsequenced_policy`, and
- `std::execution::unsequenced_policy`

have the following respective instances:

- `std::execution::seq`,
- `std::execution::par`,
- `std::execution::par_unseq`, and
- `std::execution::unseq`.

These instances are used to specify the execution policy of parallel algorithms,
i.e., the kinds of parallelism allowed.

Additional execution policies may be provided by a standard library
implementation (possible future additions may include `std::parallel::cuda` and
`std::parallel::opencl`).

### Example

```cpp
#include <algorithm>
#include <chrono>
#include <cstdint>
#include <iostream>
#include <random>
#include <vector>

#ifdef PARALLEL
#include <execution>
    namespace execution = std::execution;
#else
    enum class execution { seq, unseq, par_unseq, par };
#endif

void measure([[maybe_unused]] auto policy, std::vector<std::uint64_t> v)
{
    const auto start = std::chrono::steady_clock::now();
#ifdef PARALLEL
    std::sort(policy, v.begin(), v.end());
#else
    std::sort(v.begin(), v.end());
#endif
    const auto finish = std::chrono::steady_clock::now();
    std::cout << std::chrono::duration_cast<std::chrono::milliseconds>(finish - start)
              << '\n';
};

int main()
{
    std::vector<std::uint64_t> v(1'000'000);
    std::mt19937 gen {std::random_device{}()};
    std::ranges::generate(v, gen);

    measure(execution::seq, v);
    measure(execution::unseq, v);
    measure(execution::par_unseq, v);
    measure(execution::par, v);
}
```

Possible output:

```text
// online GNU/gcc compiler (PARALLEL macro is not defined)
81ms
80ms
79ms
78ms

// with g++ -std=c++23 -O3 ./test.cpp -ltbb -DPARALLEL
165ms
163ms
30ms
27ms
```

### See also

- **sequenced_policyparallel_policyparallel_unsequenced_policyunsequenced_policy
  (C++17)(C++17)(C++17)(C++20)** — execution policy types (class)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag*
