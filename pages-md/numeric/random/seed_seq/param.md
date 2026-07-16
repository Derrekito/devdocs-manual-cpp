# std::seed_seq::param

```cpp
template< class OutputIt >
void param( OutputIt dest ) const;  // (since C++11)
```

Outputs the initial seed sequence that's stored in the `std::seed_seq` object.

### Parameters

- **dest** — output iterator such that the expression `*dest = rt` is valid for
  a value `rt` of `result_type`

**Type requirements**

**-`OutputIt` must meet the requirements of LegacyOutputIterator.**

### Return value

(none)

### Exceptions

Throws only if an operation on `dest` throws.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <random>

int main()
{
    std::seed_seq s1 = {-1, 0, 1};
    s1.param(std::ostream_iterator<int>(std::cout, " "));
}
```

Output:

```text
-1 0 1
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/seed_seq/param*
