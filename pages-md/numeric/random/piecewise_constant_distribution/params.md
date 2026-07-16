# std::piecewise_constant_distribution<RealType>::intervals, densities

```cpp
std::vector<RealType> intervals() const;  // (1) (since C++11)
std::vector<RealType> densities() const;  // (2) (since C++11)
```

Returns the distribution parameters.

1) Returns the list of boundaries of the intervals.

2) Returns the list of probability densities of the intervals.

### Parameters

(none)

### Return value

The distribution parameters:

1) The list of boundaries of the intervals.

2) The list of probability densities of the intervals.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/numeric/random/piecewise_constant_distribution/params*
