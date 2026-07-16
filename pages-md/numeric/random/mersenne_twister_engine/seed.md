# std::mersenne_twister_engine<UIntType,w,n,m,r,a,u,d,s,b,t,c,l,f>::seed

```cpp
void seed( result_type value = default_seed );  // (1) (since C++11)
template< class Sseq >
void seed( Sseq& seq );  // (2) (since C++11)
```

Reinitializes the internal state of the random-number engine using new seed
value.

### Parameters

- **value** — seed value to use in the initialization of the internal state
- **seq** — seed sequence to use in the initialization of the internal state

### Exceptions

Throws nothing.

### Complexity

Linear in the size of the state.

### Example

```cpp
#include <iostream>
#include <random>

int main()
{
    std::mt19937 gen;

    // Seed the engine with an unsigned int
    gen.seed(1);
    std::cout << "after seed by 1: " << gen() << '\n';

    // Seed the engine with two unsigned ints
    std::seed_seq sseq{1, 2};
    gen.seed(sseq);
    std::cout << "after seed by {1,2}: " << gen() << '\n';
}
```

Possible output:

```text
after seed by 1: 1791095845
after seed by {1,2}: 3127717181
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/mersenne_twister_engine/seed*
