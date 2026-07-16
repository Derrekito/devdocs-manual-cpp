# Standard library header <valarray>

This header is part of the numeric library.

**Includes**

- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **valarray** — Numeric arrays and array slices (class template)
- **slice** — BLAS-like slice of a valarray: starting index, length, stride
  (class)
- **slice_array** — proxy to a subset of a valarray after applying a slice
  (class template)
- **gslice** — generalized slice of a valarray: starting index, set of lengths,
  set of strides (class)
- **gslice_array** — proxy to a subset of a valarray after applying a gslice
  (class template)
- **mask_array** — proxy to a subset of a valarray after applying a boolean mask
  `operator[]` (class template)
- **indirect_array** — proxy to a subset of a valarray after applying indirect
  `operator[]` (class template)

**Functions**

**Operations**

- **std::swap(std::valarray) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::begin(std::valarray) (C++11)** — overloads `std::begin` (function
  template)
- **std::end(std::valarray) (C++11)** — specializes `std::end` (function
  template)
-
  **operator+operator-operator*operator/operator%operator&operator|operator^operator<<operator>>operator&&operator||**
  — applies binary operators to each element of two valarrays, or a valarray and
  a value (function template)
- **operator==operator!=operator<operator<=operator>operator>=** — compares two
  valarrays or a valarray with a value (function template)
- **abs(std::valarray)** — applies the function `abs` to each element of
  valarray (function template)

**Exponential functions**

- **exp(std::valarray)** — applies the function `std::exp` to each element of
  valarray (function template)
- **log(std::valarray)** — applies the function `std::log` to each element of
  valarray (function template)
- **log10(std::valarray)** — applies the function `std::log10` to each element
  of valarray (function template)

**Power functions**

- **pow(std::valarray)** — applies the function `std::pow` to two valarrays or a
  valarray and a value (function template)
- **sqrt(std::valarray)** — applies the function `std::sqrt` to each element of
  valarray (function template)

**Trigonometric functions**

- **sin(std::valarray)** — applies the function `std::sin` to each element of
  valarray (function template)
- **cos(std::valarray)** — applies the function `std::cos` to each element of
  valarray (function template)
- **tan(std::valarray)** — applies the function `std::tan` to each element of
  valarray (function template)
- **asin(std::valarray)** — applies the function `std::asin` to each element of
  valarray (function template)
- **acos(std::valarray)** — applies the function `std::acos` to each element of
  valarray (function template)
- **atan(std::valarray)** — applies the function `std::atan` to each element of
  valarray (function template)
- **atan2(std::valarray)** — applies the function `std::atan2` to a valarray and
  a value (function template)

**Hyperbolic functions**

- **sinh(std::valarray)** — applies the function `std::sinh` to each element of
  valarray (function template)
- **cosh(std::valarray)** — applies the function `std::cosh` to each element of
  valarray (function template)
- **tanh(std::valarray)** — applies the function `std::tanh` to each element of
  valarray (function template)

### Synopsis

```cpp
#include <initializer_list>

namespace std {
  template<class T> class valarray;         // An array of type T
  class slice;                              // a BLAS-like slice out of an array
  template<class T> class slice_array;
  class gslice;                             // a generalized slice out of an array
  template<class T> class gslice_array;
  template<class T> class mask_array;       // a masked array
  template<class T> class indirect_array;   // an indirected array

  template<class T> void swap(valarray<T>&, valarray<T>&) noexcept;

  template<class T> valarray<T> operator* (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator* (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator* (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator/ (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator/ (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator/ (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator% (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator% (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator% (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator+ (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator+ (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator+ (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator- (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator- (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator- (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator^ (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator^ (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator^ (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator& (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator& (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator& (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator| (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator| (const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator| (const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator<<(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator<<(const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator<<(const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<T> operator>>(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> operator>>(const valarray<T>&,
                                           const typename valarray<T>::value_type&);
  template<class T> valarray<T> operator>>(const typename valarray<T>::value_type&,
                                           const valarray<T>&);

  template<class T> valarray<bool> operator&&(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator&&(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator&&(const typename valarray<T>::value_type&,
                                              const valarray<T>&);

  template<class T> valarray<bool> operator||(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator||(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator||(const typename valarray<T>::value_type&,
                                              const valarray<T>&);

  template<class T> valarray<bool> operator==(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator==(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator==(const typename valarray<T>::value_type&,
                                              const valarray<T>&);
  template<class T> valarray<bool> operator!=(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator!=(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator!=(const typename valarray<T>::value_type&,
                                              const valarray<T>&);

  template<class T> valarray<bool> operator< (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator< (const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator< (const typename valarray<T>::value_type&,
                                              const valarray<T>&);
  template<class T> valarray<bool> operator> (const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator> (const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator> (const typename valarray<T>::value_type&,
                                              const valarray<T>&);
  template<class T> valarray<bool> operator<=(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator<=(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator<=(const typename valarray<T>::value_type&,
                                              const valarray<T>&);
  template<class T> valarray<bool> operator>=(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<bool> operator>=(const valarray<T>&,
                                              const typename valarray<T>::value_type&);
  template<class T> valarray<bool> operator>=(const typename valarray<T>::value_type&,
                                              const valarray<T>&);

  template<class T> valarray<T> abs  (const valarray<T>&);
  template<class T> valarray<T> acos (const valarray<T>&);
  template<class T> valarray<T> asin (const valarray<T>&);
  template<class T> valarray<T> atan (const valarray<T>&);

  template<class T> valarray<T> atan2(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> atan2(const valarray<T>&,
                                      const typename valarray<T>::value_type&);
  template<class T> valarray<T> atan2(const typename valarray<T>::value_type&,
                                      const valarray<T>&);

  template<class T> valarray<T> cos  (const valarray<T>&);
  template<class T> valarray<T> cosh (const valarray<T>&);
  template<class T> valarray<T> exp  (const valarray<T>&);
  template<class T> valarray<T> log  (const valarray<T>&);
  template<class T> valarray<T> log10(const valarray<T>&);

  template<class T> valarray<T> pow(const valarray<T>&, const valarray<T>&);
  template<class T> valarray<T> pow(const valarray<T>&,
                                    const typename valarray<T>::value_type&);
  template<class T> valarray<T> pow(const typename valarray<T>::value_type&,
                                    const valarray<T>&);

  template<class T> valarray<T> sin  (const valarray<T>&);
  template<class T> valarray<T> sinh (const valarray<T>&);
  template<class T> valarray<T> sqrt (const valarray<T>&);
  template<class T> valarray<T> tan  (const valarray<T>&);
  template<class T> valarray<T> tanh (const valarray<T>&);

  template<class T> /* unspecified1 */ begin(valarray<T>& v);
  template<class T> /* unspecified2 */ begin(const valarray<T>& v);
  template<class T> /* unspecified1 */ end(valarray<T>& v);
  template<class T> /* unspecified2 */ end(const valarray<T>& v);
}
```

### Class template `std::valarray`

```cpp
namespace std {
  template<class T> class valarray {
  public:
    using value_type = T;

    // construct/destroy
    valarray();
    explicit valarray(size_t);
    valarray(const T&, size_t);
    valarray(const T*, size_t);
    valarray(const valarray&);
    valarray(valarray&&) noexcept;
    valarray(const slice_array<T>&);
    valarray(const gslice_array<T>&);
    valarray(const mask_array<T>&);
    valarray(const indirect_array<T>&);
    valarray(initializer_list<T>);
    ~valarray();

    // assignment
    valarray& operator=(const valarray&);
    valarray& operator=(valarray&&) noexcept;
    valarray& operator=(initializer_list<T>);
    valarray& operator=(const T&);
    valarray& operator=(const slice_array<T>&);
    valarray& operator=(const gslice_array<T>&);
    valarray& operator=(const mask_array<T>&);
    valarray& operator=(const indirect_array<T>&);

    // element access
    const T&          operator[](size_t) const;
    T&                operator[](size_t);

    // subset operations
    valarray          operator[](slice) const;
    slice_array<T>    operator[](slice);
    valarray          operator[](const gslice&) const;
    gslice_array<T>   operator[](const gslice&);
    valarray          operator[](const valarray<bool>&) const;
    mask_array<T>     operator[](const valarray<bool>&);
    valarray          operator[](const valarray<size_t>&) const;
    indirect_array<T> operator[](const valarray<size_t>&);

    // unary operators
    valarray operator+() const;
    valarray operator-() const;
    valarray operator~() const;
    valarray<bool> operator!() const;

    // compound assignment
    valarray& operator*= (const T&);
    valarray& operator/= (const T&);
    valarray& operator%= (const T&);
    valarray& operator+= (const T&);
    valarray& operator-= (const T&);
    valarray& operator^= (const T&);
    valarray& operator&= (const T&);
    valarray& operator|= (const T&);
    valarray& operator<<=(const T&);
    valarray& operator>>=(const T&);

    valarray& operator*= (const valarray&);
    valarray& operator/= (const valarray&);
    valarray& operator%= (const valarray&);
    valarray& operator+= (const valarray&);
    valarray& operator-= (const valarray&);
    valarray& operator^= (const valarray&);
    valarray& operator|= (const valarray&);
    valarray& operator&= (const valarray&);
    valarray& operator<<=(const valarray&);
    valarray& operator>>=(const valarray&);

    // member functions
    void swap(valarray&) noexcept;

    size_t size() const;

    T sum() const;
    T min() const;
    T max() const;

    valarray shift (int) const;
    valarray cshift(int) const;
    valarray apply(T func(T)) const;
    valarray apply(T func(const T&)) const;
    void resize(size_t sz, T c = T());
  };

  template<class T, size_t cnt> valarray(const T(&)[cnt], size_t) -> valarray<T>;
}
```

### Class `std::slice`

```cpp
namespace std {
  class slice {
  public:
    slice();
    slice(size_t, size_t, size_t);

    size_t start() const;
    size_t size() const;
    size_t stride() const;

    friend bool operator==(const slice& x, const slice& y);
  };
}
```

### Class template `std::slice_array`

```cpp
namespace std {
  template<class T> class slice_array {
  public:
    using value_type = T;

    void operator=  (const valarray<T>&) const;
    void operator*= (const valarray<T>&) const;
    void operator/= (const valarray<T>&) const;
    void operator%= (const valarray<T>&) const;
    void operator+= (const valarray<T>&) const;
    void operator-= (const valarray<T>&) const;
    void operator^= (const valarray<T>&) const;
    void operator&= (const valarray<T>&) const;
    void operator|= (const valarray<T>&) const;
    void operator<<=(const valarray<T>&) const;
    void operator>>=(const valarray<T>&) const;

    slice_array(const slice_array&);
    ~slice_array();
    const slice_array& operator=(const slice_array&) const;
    void operator=(const T&) const;

    slice_array() = delete;     // as implied by declaring copy constructor above
  };
}
```

### Class `std::gslice`

```cpp
namespace std {
  class gslice {
  public:
    gslice();
    gslice(size_t s, const valarray<size_t>& l, const valarray<size_t>& d);

    size_t           start() const;
    valarray<size_t> size() const;
    valarray<size_t> stride() const;
  };
}
```

### Class template `std::gslice_array`

```cpp
namespace std {
  template<class T> class gslice_array {
  public:
    using value_type = T;

    void operator=  (const valarray<T>&) const;
    void operator*= (const valarray<T>&) const;
    void operator/= (const valarray<T>&) const;
    void operator%= (const valarray<T>&) const;
    void operator+= (const valarray<T>&) const;
    void operator-= (const valarray<T>&) const;
    void operator^= (const valarray<T>&) const;
    void operator&= (const valarray<T>&) const;
    void operator|= (const valarray<T>&) const;
    void operator<<=(const valarray<T>&) const;
    void operator>>=(const valarray<T>&) const;

    gslice_array(const gslice_array&);
    ~gslice_array();
    const gslice_array& operator=(const gslice_array&) const;
    void operator=(const T&) const;

    gslice_array() = delete;    // as implied by declaring copy constructor above
  };
}
```

### Class template `std::mask_array`

```cpp
namespace std {
  template<class T> class mask_array {
  public:
    using value_type = T;

    void operator=  (const valarray<T>&) const;
    void operator*= (const valarray<T>&) const;
    void operator/= (const valarray<T>&) const;
    void operator%= (const valarray<T>&) const;
    void operator+= (const valarray<T>&) const;
    void operator-= (const valarray<T>&) const;
    void operator^= (const valarray<T>&) const;
    void operator&= (const valarray<T>&) const;
    void operator|= (const valarray<T>&) const;
    void operator<<=(const valarray<T>&) const;
    void operator>>=(const valarray<T>&) const;

    mask_array(const mask_array&);
    ~mask_array();
    const mask_array& operator=(const mask_array&) const;
    void operator=(const T&) const;

    mask_array() = delete;      // as implied by declaring copy constructor above
  };
}
```

### Class template `std::indirect_array`

```cpp
namespace std {
  template<class T> class indirect_array {
  public:
    using value_type = T;

    void operator=  (const valarray<T>&) const;
    void operator*= (const valarray<T>&) const;
    void operator/= (const valarray<T>&) const;
    void operator%= (const valarray<T>&) const;
    void operator+= (const valarray<T>&) const;
    void operator-= (const valarray<T>&) const;
    void operator^= (const valarray<T>&) const;
    void operator&= (const valarray<T>&) const;
    void operator|= (const valarray<T>&) const;
    void operator<<=(const valarray<T>&) const;
    void operator>>=(const valarray<T>&) const;

    indirect_array(const indirect_array&);
    ~indirect_array();
    const indirect_array& operator=(const indirect_array&) const;
    void operator=(const T&) const;

    indirect_array() = delete;  // as implied by declaring copy constructor above
  };
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 125 | C++98 | the return type of `valarray<T>::operator!()` was
      `valarray<T>` in the synopsis | corrected to `valarray<bool>`

---
*Source: https://en.cppreference.com/w/cpp/header/valarray*
