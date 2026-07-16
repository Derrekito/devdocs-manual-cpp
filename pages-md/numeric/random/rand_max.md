# RAND_MAX

```cpp
#define RAND_MAX /*implementation defined*/
```

Expands to an integer constant expression equal to the maximum value returned by
the function `std::rand`. This value is implementation dependent. It's
guaranteed that this value is at least `32767`.

### Example

```cpp
#include <climits>
#include <cstdlib>
#include <ctime>
#include <iostream>

int main()
{
    // use current time as seed for random generator
    std::srand(std::time(NULL));

    std::cout << "RAND_MAX: " << RAND_MAX << '\n'
              << "INT_MAX: " << INT_MAX << '\n'
              << "Random value on [0,1]: "
              << static_cast<double>(std::rand()) / RAND_MAX << '\n';
}
```

Possible output:

```text
RAND_MAX: 2147483647
INT_MAX: 2147483647
Random value on [0,1]: 0.618608
```

### See also

- **rand** — generates a pseudo-random number (function)
- **srand** — seeds pseudo-random number generator (function)

**C documentation for `RAND_MAX`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/RAND_MAX*
