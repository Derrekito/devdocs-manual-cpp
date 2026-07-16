# std::negative_binomial_distribution<IntType>::p, k

```cpp
double p() const;  // (1) (since C++11)
IntType k() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the p distribution parameter. It defines the probability of a trial
   generating `true`. The default value is `0.5`.

2) Returns the k distribution parameter. It defines the number of desired
   outcomes. The default value is `1`.

### Parameters

(none)

### Return value

1) The p distribution parameter.

2) The k distribution parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/negative_binomial_distribution/params*
