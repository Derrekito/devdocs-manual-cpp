# std::binomial_distribution<IntType>::p, t

```cpp
double p() const;  // (1) (since C++11)
IntType t() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the p distribution parameter. It defines the probability of a trial
   generating `true`. The default value is `0.5`.

2) Returns the t distribution parameter. It identifies the number of trials. The
   default value is `1`.

### Parameters

(none)

### Return value

1) The p distribution parameter.

2) The t distribution parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/binomial_distribution/params*
