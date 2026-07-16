# std::generate_canonical

```cpp
template< class RealType, std::size_t Bits, class Generator >
RealType generate_canonical( Generator& g );  // (since C++11)
```

Generates a random floating point number in range `[``​0​``,``1``)`.

To generate enough entropy, `generate_canonical()` will call `g()` exactly
\(\small k\)k times, where \(\small k = \max(1, \lceil \frac{b}{\log_2 R}
\rceil)\)k = max(1, ⌈ b / log2 R ⌉) and

- `b = std::min(Bits, std::size_t {std::numeric_limits<RealType>::digits})`,
- `R = g.max() - g.min() + 1`.

### Parameters

- **g** — generator to use to acquire entropy

### Return value

Floating point value in range `[``​0​``,``1``)`.

### Exceptions

None except from those thrown by `g`.

### Notes

Some existing implementations have a bug where they may occasionally return
`1.0` if `RealType` is `float` GCC #63176 LLVM #18767 MSVC STL #1074. This is
LWG issue 2524.

### Example

Produce random numbers with 10 bits of randomness: this may produce only k * R
distinct values.

```cpp
#include <iostream>
#include <random>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    for (int n = 0; n < 10; ++n)
        std::cout << std::generate_canonical<double, 10>(gen) << ' ';
    std::cout << '\n';
}
```

Possible output:

```text
0.208143 0.824147 0.0278604 0.343183 0.0173263 0.864057 0.647037 0.539467 0.0583497 0.609219
```

### See also

- **uniform_real_distribution (C++11)** — produces real values evenly
  distributed across a range (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/generate_canonical*
