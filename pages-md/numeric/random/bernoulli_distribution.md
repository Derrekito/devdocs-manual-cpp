# std::bernoulli_distribution

```cpp
class bernoulli_distribution;  // (since C++11)
```

Produces random boolean values, according to the discrete probability function.
The probability of true is P(b|p) =

⎧
⎨
⎩p, if b is `true`
1 − p, if b is `false`

`std::bernoulli_distribution` satisfies RandomNumberDistribution.

### Member types

- **`result_type` (C++11)** — bool
- **`param_type` (C++11)** — the type of the parameter set, see
  RandomNumberDistribution.

### Member functions

- **(constructor) (C++11)** — constructs new distribution (public member
  function)
- **reset (C++11)** — resets the internal state of the distribution (public
  member function)

**Generation**

- **operator() (C++11)** — generates the next random number in the distribution
  (public member function)

**Characteristics**

- **p (C++11)** — returns the *p* distribution parameter (probability of
  generating true) (public member function)
- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)
- **min (C++11)** — returns the minimum potentially generated value (public
  member function)
- **max (C++11)** — returns the maximum potentially generated value (public
  member function)

### Non-member functions

- **operator==operator!= (C++11)(C++11)(removed in C++20)** — compares two
  distribution objects (function)
- **operator<<operator>> (C++11)** — performs stream input and output on
  pseudo-random number distribution (function template)

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <map>
#include <random>
#include <string>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    // give "true" 1/4 of the time
    // give "false" 3/4 of the time
    std::bernoulli_distribution d(0.25);

    std::map<bool, int> hist;
    for (int n = 0; n < 10000; ++n)
        ++hist[d(gen)];

    std::cout << std::boolalpha;
    for (auto const& [key, value] : hist)
        std::cout << std::setw(5) << key << ' '
                  << std::string(value / 500, '*') << '\n';
}
```

Possible output:

```text
false ***************
 true ****
```

### External links

  Weisstein, Eric W. "Bernoulli Distribution." From MathWorld — A Wolfram Web
      Resource.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/bernoulli_distribution*
