# std::seed_seq

```cpp
class seed_seq;  // (since C++11)
```

`std::seed_seq` consumes a sequence of integer-valued data and produces a
requested number of unsigned integer values `i`, 0 ≤ i < 232, based on the
consumed data. The produced values are distributed over the entire 32-bit range
even if the consumed values are close.

It provides a way to seed a large number of random number engines or to seed a
generator that requires a lot of entropy, given a small seed or a poorly
distributed initial seed sequence.

`std::seed_seq` meets the requirements of SeedSequence.

### Member types

- **`result_type` (C++11)** — `std::uint_least32_t`

### Member functions

- **(constructor) (C++11)** — constructs and seeds the std::seed_seq object
  (public member function)
- **operator= [deleted] (C++11)** — `seed_seq` is not assignable (public member
  function)
- **generate (C++11)** — calculates the bias-eliminated, evenly distributed
  32-bit values (public member function)
- **size (C++11)** — obtains the number of 32-bit values stored in std::seed_seq
  (public member function)
- **param (C++11)** — obtains the 32-bit values stored in std::seed_seq (public
  member function)

### Example

```cpp
#include <cstdint>
#include <iostream>
#include <random>

int main()
{
    std::seed_seq seq{1, 2, 3, 4, 5};
    std::vector<std::uint32_t> seeds(10);
    seq.generate(seeds.begin(), seeds.end());
    for (std::uint32_t n : seeds)
        std::cout << n << '\n';
}
```

Possible output:

```text
4204997637
4246533866
1856049002
1129615051
690460811
1075771511
46783058
3904109078
1534123438
1495905678
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/seed_seq*
