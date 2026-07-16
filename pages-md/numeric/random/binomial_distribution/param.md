# std::binomial_distribution<IntType>::param

```cpp
param_type param() const;  // (1) (since C++11)
void param( const param_type& params );  // (2) (since C++11)
```

Manages the associated distribution parameter set.

1) Returns the associated parameter set.

2) Sets the associated parameter set to `params`.

### Parameters

- **params** — new contents of the associated parameter set

### Return value

1) The associated parameter set.

2) (none)

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <random>

int main()
{
    std::random_device rd;
    std::mt19937 gen(rd());
    using BinomialDist = std::binomial_distribution<>;
    BinomialDist bino_dis(1, 0.5);

    std::cout << "A sample of Binomial(  1, 0.5): " << bino_dis(gen) << '\n';

    // Use another parameter set
    bino_dis.param(BinomialDist::param_type(100,0.9));
    std::cout << "A sample of Binomial(100, 0.9): " << bino_dis(gen) << '\n';
}
```

Possible output:

```text
A sample of Binomial(  1, 0.5): 0
A sample of Binomial(100, 0.9): 94
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/binomial_distribution/param*
