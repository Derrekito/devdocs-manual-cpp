# std::gamma_distribution<RealType>::alpha, beta

```cpp
RealType alpha() const;  // (1) (since C++11)
RealType beta() const;  // (2) (since C++11)
```

Returns the distribution parameters the distribution has been constructed with.

1) Returns the α distribution parameter. It is also known as the shape
   parameter. The default value is `1.0`.

2) Returns the β distribution parameter. It is also known as the scale
   parameter. The default value is `1.0`.

### Parameters

(none)

### Return value

1) Floating point value identifying the α parameter.

2) Floating point value identifying the β parameter.

### See also

- **param (C++11)** — gets or sets the distribution parameter object (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/gamma_distribution/params*
