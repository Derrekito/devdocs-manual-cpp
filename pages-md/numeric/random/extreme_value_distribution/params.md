# std::extreme_value_distribution<RealType>::a, b

```cpp
RealType a() const;  // (1) (since C++11)
RealType b() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the a distribution parameter (location). The default value is `1.0`.

2) Returns the b distribution parameter (scale). The default value is `0.0`.

### Parameters

(none)

### Return value

1) The a distribution parameter (location).

2) The b distribution parameter (scale).

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/extreme_value_distribution/params*
