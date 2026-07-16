# std::random_device::entropy

```cpp
double entropy() const noexcept;  // (since C++11)
```

Obtains an estimate of the random number device entropy, which is a
floating-point value between 0 and log2(max()+1) (which is equal to
`std::numeric_limits<unsigned int>::digits`). If the device has n states whose
individual probabilities are P0,...,Pn-1, the device entropy S is defined as

S = −∑n-1i=0 Pilog(Pi)

A deterministic random number generator (e.g. a pseudo-random engine) has
entropy zero.

### Return value

The value of the device entropy, or zero if not applicable.

### Notes

This function is not fully implemented in some standard libraries. For example,
LLVM libc++ prior to version 12 always returns zero even though the device is
non-deterministic. In comparison, Microsoft Visual C++ implementation always
returns `32`, and boost.random returns `10`.

The entropy of the Linux kernel device `/dev/urandom` may be obtained using
`ioctl RNDGETENTCNT` — that is what `std::random_device::entropy()` in GNU
libstdc++ uses as of version 8.1.

### Example

Example output on one of the implementations

```cpp
#include <iostream>
#include <random>

int main()
{
    std::random_device rd;
    std::cout << rd.entropy() << '\n';
}
```

Possible output:

```text
32
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/random_device/entropy*
