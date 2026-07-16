# Standard library header <bit> (C++20)

This header is part of the numeric library.

**Types**

- **endian (C++20)** — indicates the endianness of scalar types (enum)

**Functions**

- **bit_cast (C++20)** — reinterpret the object representation of one type as
  that of another (function template)
- **byteswap (C++23)** — reverses the bytes in the given integer value (function
  template)
- **has_single_bit (C++20)** — checks if a number is an integral power of `2`
  (function template)
- **bit_ceil (C++20)** — finds the smallest integral power of two not less than
  the given value (function template)
- **bit_floor (C++20)** — finds the largest integral power of two not greater
  than the given value (function template)
- **bit_width (C++20)** — finds the smallest number of bits needed to represent
  the given value (function template)
- **rotl (C++20)** — computes the result of bitwise left-rotation (function
  template)
- **rotr (C++20)** — computes the result of bitwise right-rotation (function
  template)
- **countl_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the most significant bit (function template)
- **countl_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the most significant bit (function template)
- **countr_zero (C++20)** — counts the number of consecutive `​0​` bits,
  starting from the least significant bit (function template)
- **countr_one (C++20)** — counts the number of consecutive `1` bits, starting
  from the least significant bit (function template)
- **popcount (C++20)** — counts the number of `1` bits in an unsigned integer
  (function template)

### Synopsis

```cpp
namespace std {
  // bit_cast
  template<class To, class From>
    constexpr To bit_cast(const From& from) noexcept;

  // byteswap
  template <class T>
    constexpr T byteswap(T value) noexcept;

  // integral powers of 2
  template<class T>
    constexpr bool has_single_bit(T x) noexcept;
  template<class T>
    constexpr T bit_ceil(T x);
  template<class T>
    constexpr T bit_floor(T x) noexcept;
  template<class T>
    constexpr int bit_width(T x) noexcept;

  // rotating
  template<class T>
    [[nodiscard]] constexpr T rotl(T x, int s) noexcept;
  template<class T>
    [[nodiscard]] constexpr T rotr(T x, int s) noexcept;

  // counting
  template<class T>
    constexpr int countl_zero(T x) noexcept;
  template<class T>
    constexpr int countl_one(T x) noexcept;
  template<class T>
    constexpr int countr_zero(T x) noexcept;
  template<class T>
    constexpr int countr_one(T x) noexcept;
  template<class T>
    constexpr int popcount(T x) noexcept;

  // endian
  enum class endian {
    little = /* see description */,
    big    = /* see description */,
    native = /* see description */
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/bit*
