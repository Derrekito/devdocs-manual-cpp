# std::chrono::month::month

```cpp
month() = default;  // (1) (since C++20)
constexpr explicit month( unsigned m ) noexcept;  // (2) (since C++20)
```

Constructs a `month` object.

1) Default constructor leaves the month value uninitialized.

2) If `m <= 255`, constructs a `month` object holding the month value `m`.
   Otherwise the value held is unspecified.

---
*Source: https://en.cppreference.com/w/cpp/chrono/month/month*
