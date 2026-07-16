# Compile-time rational arithmetic (since C++11)

The class template `std::ratio` and associated templates provide compile-time
rational arithmetic support. Each instantiation of this template exactly
represents any finite rational number.

### Compile-time fractions

- **ratio (C++11)** — represents exact rational fraction (class template)

The following convenience typedefs that correspond to the SI ratios are provided
by the standard library:

- **`quecto` (since C++26)** — std::ratio<1,
  1000000000000000000000000000000>(10-30)[1]
- **`ronto` (since C++26)** — std::ratio<1,
  1000000000000000000000000000>(10-27)[1]
- **`yocto`** — std::ratio<1, 1000000000000000000000000>(10-24)[1]
- **`zepto`** — std::ratio<1, 1000000000000000000000>(10-21)[1]
- **`atto`** — std::ratio<1, 1000000000000000000>(10-18)
- **`femto`** — std::ratio<1, 1000000000000000>(10-15)
- **`pico`** — std::ratio<1, 1000000000000>(10-12)
- **`nano`** — std::ratio<1, 1000000000>(10-9)
- **`micro`** — std::ratio<1, 1000000>(10-6)
- **`milli`** — std::ratio<1, 1000>(10-3)
- **`centi`** — std::ratio<1, 100>(10-2)
- **`deci`** — std::ratio<1, 10>(10-1)
- **`deca`** — std::ratio<10, 1>(101)
- **`hecto`** — std::ratio<100, 1>(102)
- **`kilo`** — std::ratio<1000, 1>(103)
- **`mega`** — std::ratio<1000000, 1>(106)
- **`giga`** — std::ratio<1000000000, 1>(109)
- **`tera`** — std::ratio<1000000000000, 1>(1012)
- **`peta`** — std::ratio<1000000000000000, 1>(1015)
- **`exa`** — std::ratio<1000000000000000000, 1>(1018)
- **`zetta`** — std::ratio<1000000000000000000000, 1>(1021)[2]
- **`yotta`** — std::ratio<1000000000000000000000000, 1>(1024)[2]
- **`ronna` (since C++26)** — std::ratio<1000000000000000000000000000,
  1>(1027)[2]
- **`quetta` (since C++26)** — std::ratio<1000000000000000000000000000000,
  1>(1030)[2]

1. These typedefs are only defined if `std::intmax_t` can represent the
   denominator.
1. These typedefs are only defined if `std::intmax_t` can represent the
   numerator.

### Compile-time rational arithmetic

Several alias templates, that perform arithmetic operations on `ratio` objects
at compile-time are provided.

- **ratio_add (C++11)** — adds two `ratio` objects at compile-time (alias
  template)
- **ratio_subtract (C++11)** — subtracts two `ratio` objects at compile-time
  (alias template)
- **ratio_multiply (C++11)** — multiplies two `ratio` objects at compile-time
  (alias template)
- **ratio_divide (C++11)** — divides two `ratio` objects at compile-time (alias
  template)

### Compile-time rational comparison

Several class templates, that perform comparison operations on `ratio` objects
at compile-time are provided.

- **ratio_equal (C++11)** — compares two `ratio` objects for equality at
  compile-time (class template)
- **ratio_not_equal (C++11)** — compares two `ratio` objects for inequality at
  compile-time (class template)
- **ratio_less (C++11)** — compares two `ratio` objects for *less than* at
  compile-time (class template)
- **ratio_less_equal (C++11)** — compares two `ratio` objects for *less than or
  equal to* at compile-time (class template)
- **ratio_greater (C++11)** — compares two `ratio` objects for *greater than* at
  compile-time (class template)
- **ratio_greater_equal (C++11)** — compares two `ratio` objects for *greater
  than or equal to* at compile-time (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio*
