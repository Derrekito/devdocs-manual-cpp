# std::binomial_distribution<IntType>::operator()

```cpp
template< class Generator >
result_type operator()( Generator& g );  // (1) (since C++11)
template< class Generator >
result_type operator()( Generator& g, const param_type& params );  // (2) (since C++11)
```

Generates random numbers that are distributed according to the associated
probability function. The entropy is acquired by calling `g.operator()`.

The first version uses the associated parameter set, the second version uses
`params`. The associated parameter set is not modified.

### Parameters

- **g** — a uniform random bit generator object
- **params** — distribution parameter set to use instead of the associated one

**Type requirements**

**-`Generator` must meet the requirements of UniformRandomBitGenerator.**

### Return value

The generated random number.

### Complexity

Amortized constant number of invocations of `g.operator()`.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/binomial_distribution/operator()*
