# std::normal_distribution<RealType>::mean, stddev

```cpp
RealType mean() const;  // (1) (since C++11)
RealType stddev() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the mean μ distribution parameter. The mean specifies the location of
   the peak. The default value is `0.0`.

2) Returns the deviation σ distribution parameter. The default value is `1.0`.

### Parameters

(none)

### Return value

1) The mean μ distribution parameter.

2) The deviation σ distribution parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/normal_distribution/params*
