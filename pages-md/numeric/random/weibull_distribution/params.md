# std::weibull_distribution<RealType>::a, b

```cpp
RealType a() const;  // (1) (since C++11)
RealType b() const;  // (2) (since C++11)
```

Returns the parameters the distribution was constructed with.

1) Returns the a parameter. It defines the shape of the distribution. The
   default value is `1.0`.

2) Returns the b parameter. It defines the scale of the distribution. The
   default value is `1.0`.

### Parameters

(none)

### Return value

1) The value of the a parameter.

2) The value of the b parameter.

### Complexity

Constant.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/weibull_distribution/params*
