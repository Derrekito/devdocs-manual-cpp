# std::lognormal_distribution<RealType>::m, s

```cpp
RealType m() const;  // (1) (since C++11)
RealType s() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the log-mean m distribution parameter. It defines the location of the
   peak. The default value is `0.0`.

2) Returns the log-deviation s distribution parameter. The default value is
   `1.0`.

### Parameters

(none)

### Return value

1) The log-mean m distribution parameter.

2) The log-deviation s distribution parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/lognormal_distribution/params*
