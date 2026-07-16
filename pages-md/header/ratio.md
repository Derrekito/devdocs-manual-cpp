# Standard library header <ratio> (C++11)

This header is part of the compile-time rational arithmetic library.

**Classes**

- **ratio (C++11)** — represents exact rational fraction (class template)

**Arithmetic**

- **ratio_add (C++11)** — adds two `ratio` objects at compile-time (alias
  template)
- **ratio_subtract (C++11)** — subtracts two `ratio` objects at compile-time
  (alias template)
- **ratio_multiply (C++11)** — multiplies two `ratio` objects at compile-time
  (alias template)
- **ratio_divide (C++11)** — divides two `ratio` objects at compile-time (alias
  template)

**Comparison**

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

**Type Aliases**

- **`quecto` (C++26)** — `std::ratio<1, 1000000000000000000000000000000>`, if
  `std::intmax_t` can represent the denominator
- **`ronto` (C++26)** — `std::ratio<1, 1000000000000000000000000000>`, if
  `std::intmax_t` can represent the denominator
- **`yocto`** — `std::ratio<1, 1000000000000000000000000>`, if `std::intmax_t`
  can represent the denominator
- **`zepto`** — `std::ratio<1, 1000000000000000000000>`, if `std::intmax_t` can
  represent the denominator
- **`atto`** — `std::ratio<1, 1000000000000000000>`
- **`femto`** — `std::ratio<1, 1000000000000000>`
- **`pico`** — `std::ratio<1, 1000000000000>`
- **`nano`** — `std::ratio<1, 1000000000>`
- **`micro`** — `std::ratio<1, 1000000>`
- **`milli`** — `std::ratio<1, 1000>`
- **`centi`** — `std::ratio<1, 100>`
- **`deci`** — `std::ratio<1, 10>`
- **`deca`** — `std::ratio<10, 1>`
- **`hecto`** — `std::ratio<100, 1>`
- **`kilo`** — `std::ratio<1000, 1>`
- **`mega`** — `std::ratio<1000000, 1>`
- **`giga`** — `std::ratio<1000000000, 1>`
- **`tera`** — `std::ratio<1000000000000, 1>`
- **`peta`** — `std::ratio<1000000000000000, 1>`
- **`exa`** — `std::ratio<1000000000000000000, 1>`
- **`zetta`** — `std::ratio<1000000000000000000000, 1>`, if `std::intmax_t` can
  represent the numerator
- **`yotta`** — `std::ratio<1000000000000000000000000, 1>`, if `std::intmax_t`
  can represent the numerator
- **`ronna` (C++26)** — `std::ratio<1000000000000000000000000000, 1>`, if
  `std::intmax_t` can represent the numerator
- **`quetta` (C++26)** — `std::ratio<1000000000000000000000000000000, 1>`, if
  `std::intmax_t` can represent the numerator

### Synopsis

```cpp
namespace std {

    // class template ratio
    template <intmax_t N, intmax_t D = 1>
    class ratio {
    public:
        typedef ratio<num, den> type;
        static constexpr intmax_t num;
        static constexpr intmax_t den;
    };

    // ratio arithmetic
    template <class R1, class   R2> using   ratio_add      = /*ratio*/;
    template <class R1, class   R2> using   ratio_subtract = /*ratio*/;
    template <class R1, class   R2> using   ratio_multiply = /*ratio*/;
    template <class R1, class   R2> using   ratio_divide   = /*ratio*/;

    // ratio comparison
    template <class R1, class R2> struct ratio_equal;
    template <class R1, class R2> struct ratio_not_equal;
    template <class R1, class R2> struct ratio_less;
    template <class R1, class R2> struct ratio_less_equal;
    template <class R1, class R2> struct ratio_greater;
    template <class R1, class R2> struct ratio_greater_equal;

    // convenience SI typedefs
    typedef ratio<1, 1000000000000000000000000000000> quecto;
    typedef ratio<1,    1000000000000000000000000000> ronto;
    typedef ratio<1,       1000000000000000000000000> yocto;
    typedef ratio<1,          1000000000000000000000> zepto;
    typedef ratio<1,             1000000000000000000> atto;
    typedef ratio<1,                1000000000000000> femto;
    typedef ratio<1,                   1000000000000> pico;
    typedef ratio<1,                      1000000000> nano;
    typedef ratio<1,                         1000000> micro;
    typedef ratio<1,                            1000> milli;
    typedef ratio<1,                             100> centi;
    typedef ratio<1,                              10> deci;
    typedef ratio<                             10, 1> deca;
    typedef ratio<                            100, 1> hecto;
    typedef ratio<                           1000, 1> kilo;
    typedef ratio<                        1000000, 1> mega;
    typedef ratio<                     1000000000, 1> giga;
    typedef ratio<                  1000000000000, 1> tera;
    typedef ratio<               1000000000000000, 1> peta;
    typedef ratio<            1000000000000000000, 1> exa;
    typedef ratio<         1000000000000000000000, 1> zetta;
    typedef ratio<      1000000000000000000000000, 1> yotta;
    typedef ratio<   1000000000000000000000000000, 1> ronna;
    typedef ratio<1000000000000000000000000000000, 1> quetta;

}
```

---
*Source: https://en.cppreference.com/w/cpp/header/ratio*
