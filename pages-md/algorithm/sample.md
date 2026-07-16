# std::sample

```cpp
template< class PopulationIterator, class SampleIterator,
          class Distance, class URBG >
SampleIterator sample( PopulationIterator first, PopulationIterator last,
                       SampleIterator out, Distance n,
                       URBG&& g );  // (since C++17)
```

Selects `n` elements from the sequence `[``first``,``last``)` (without
replacement) such that each possible sample has equal probability of appearance,
and writes those selected elements into the output iterator `out`. Random
numbers are generated using the random number generator `g`.

If `n` is greater than the number of elements in the sequence, selects `last -
first` elements.

The algorithm is stable (preserves the relative order of the selected elements)
only if `PopulationIterator` meets the requirements of LegacyForwardIterator.

The behavior is undefined if `out` is in `[``first``,``last``)`.

### Parameters

- **first, last** — pair of iterators forming the range from which to make the
  sampling (the population)
- **out** — the output iterator where the samples are written
- **n** — number of samples to make
- **g** — the random number generator used as the source of randomness

**Type requirements**

**-`PopulationIterator` must meet the requirements of LegacyInputIterator.**

**-`SampleIterator` must meet the requirements of LegacyOutputIterator.**

**-`SampleIterator` must also meet the requirements of LegacyRandomAccessIterator if `PopulationIterator` does not meet LegacyForwardIterator**

**-`PopulationIterator`'s value type must be writable to `out`**

**-`Distance` must be an integer type**

**-`std::remove_reference_t<URBG>` must meet the requirements of UniformRandomBitGenerator and its return type must be convertible to `Distance`**

### Return value

Returns a copy of `out` after the last sample that was output, that is, end of
the sample range.

### Complexity

Linear in `std::distance(first, last)`.

### Notes

This function may implement selection sampling or reservoir sampling.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_sample` | 201603L | (C++17) | `std::sample`

### Possible implementation

See the implementations in libstdc++, libc++ and MSVC STL.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <random>
#include <string>

int main()
{
    std::string in {"ABCDEFGHIJK"}, out;
    std::sample(in.begin(), in.end(), std::back_inserter(out), 4,
                std::mt19937 {std::random_device{}()});
    std::cout << "Four random letters out of " << in << " : " << out << '\n';
}
```

Possible output:

```text
Four random letters out of ABCDEFGHIJK: EFGK
```

### See also

- **random_shuffleshuffle (until C++17)(C++11)** — randomly re-orders elements
  in a range (function template)
- **ranges::sample (C++20)** — selects N random elements from a sequence
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/sample*
