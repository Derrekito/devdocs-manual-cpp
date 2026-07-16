# std::valarray

```cpp
template< class T >
class valarray;
```

`std::valarray` is the class for representing and manipulating arrays of values.
It supports element-wise mathematical operations and various forms of
generalized subscript operators, slicing and indirect access.

### Notes

`std::valarray` and helper classes are defined to be free of certain forms of
aliasing, thus allowing operations on these classes to be optimized similar to
the effect of the keyword `restrict` in the C programming language. In addition,
functions and operators that take `valarray` arguments are allowed to return
proxy objects to make it possible for the compiler to optimize an expression
such as `v1 = a * v2 + v3;` as a single loop that executes `v1[i] = a * v2[i] +
v3[i];` avoiding any temporaries or multiple passes. However, expression
templates make the same optimization technique available for any C++ container,
and the majority of numeric libraries prefer expression templates to valarrays
for flexibility. Some C++ standard library implementations use expression
templates to implement efficient operations on `std::valarray` (e.g. GNU
libstdc++ and LLVM libc++). Only rarely are valarrays optimized any further, as
in e.g. Intel Integrated Performance Primitives.

### Template parameters

- **T** — the type of the elements. The type must meet the NumericType
  requirements

### Member types

- **`value_type`** — `T`

### Member functions

- **(constructor)** — constructs new numeric array (public member function)
- **(destructor)** — destructs the numeric array (public member function)
- **operator=** — assigns the contents (public member function)
- **operator[]** — get/set valarray element, slice, or mask (public member
  function)
- **operator+operator-operator~operator!** — applies a unary arithmetic operator
  to each element of the valarray (public member function)
-
  **operator+=operator-=operator*=operator/=operator%=operator&=operator|=operator^=operator<<=operator>>=**
  — applies compound assignment operator to each element of the valarray (public
  member function)
- **swap** — swaps with another valarray (public member function)
- **size** — returns the size of valarray (public member function)
- **resize** — changes the size of valarray (public member function)
- **sum** — calculates the sum of all elements (public member function)
- **min** — returns the smallest element (public member function)
- **max** — returns the largest element (public member function)
- **shift** — zero-filling shift the elements of the valarray (public member
  function)
- **cshift** — circular shift of the elements of the valarray (public member
  function)
- **apply** — applies a function to every element of a valarray (public member
  function)

### Non-member functions

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

### Helper classes

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

### Deduction guides(since C++17)

### See also

- **simd (parallelism TS v2)** — data-parallel vector type (class template)
- **simd_mask (parallelism TS v2)** — data-parallel type with the element type
  bool (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray*
