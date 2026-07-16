# Mathematical special functions (since C++17)

The Mathematical Special Functions library was originally part of Library TR1
ISO/IEC TR 19768:2007, then published as an independent ISO standard, ISO/IEC
29124:2010, and finally merged to ISO C++ as of C++17.

See Mathematical special functions for the ISO/IEC 29124:2010 version of this
library.

### Functions

- **assoc_laguerreassoc_laguerrefassoc_laguerrel (C++17)(C++17)(C++17)** —
  associated Laguerre polynomials (function)
- **assoc_legendreassoc_legendrefassoc_legendrel (C++17)(C++17)(C++17)** —
  associated Legendre polynomials (function)
- **betabetafbetal (C++17)(C++17)(C++17)** — beta function (function)
- **comp_ellint_1comp_ellint_1fcomp_ellint_1l (C++17)(C++17)(C++17)** —
  (complete) elliptic integral of the first kind (function)
- **comp_ellint_2comp_ellint_2fcomp_ellint_2l (C++17)(C++17)(C++17)** —
  (complete) elliptic integral of the second kind (function)
- **comp_ellint_3comp_ellint_3fcomp_ellint_3l (C++17)(C++17)(C++17)** —
  (complete) elliptic integral of the third kind (function)
- **cyl_bessel_icyl_bessel_ifcyl_bessel_il (C++17)(C++17)(C++17)** — regular
  modified cylindrical Bessel functions (function)
- **cyl_bessel_jcyl_bessel_jfcyl_bessel_jl (C++17)(C++17)(C++17)** — cylindrical
  Bessel functions (of the first kind) (function)
- **cyl_bessel_kcyl_bessel_kfcyl_bessel_kl (C++17)(C++17)(C++17)** — irregular
  modified cylindrical Bessel functions (function)
- **cyl_neumanncyl_neumannfcyl_neumannl (C++17)(C++17)(C++17)** — cylindrical
  Neumann functions (function)
- **ellint_1ellint_1fellint_1l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the first kind (function)
- **ellint_2ellint_2fellint_2l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the second kind (function)
- **ellint_3ellint_3fellint_3l (C++17)(C++17)(C++17)** — (incomplete) elliptic
  integral of the third kind (function)
- **expintexpintfexpintl (C++17)(C++17)(C++17)** — exponential integral
  (function)
- **hermitehermitefhermitel (C++17)(C++17)(C++17)** — Hermite polynomials
  (function)
- **legendrelegendreflegendrel (C++17)(C++17)(C++17)** — Legendre polynomials
  (function)
- **laguerrelaguerreflaguerrel (C++17)(C++17)(C++17)** — Laguerre polynomials
  (function)
- **riemann_zetariemann_zetafriemann_zetal (C++17)(C++17)(C++17)** — Riemann
  zeta function (function)
- **sph_besselsph_besselfsph_bessell (C++17)(C++17)(C++17)** — spherical Bessel
  functions (of the first kind) (function)
- **sph_legendresph_legendrefsph_legendrel (C++17)(C++17)(C++17)** — spherical
  associated Legendre functions (function)
- **sph_neumannsph_neumannfsph_neumannl (C++17)(C++17)(C++17)** — spherical
  Neumann functions (function)

### Notes

The float and long double overloads for math special functions without the "`f`"
or "`l`" suffix are present in the final draft of ISO/IEC 29124:2010 (N3060),
but absent in the published C++17/20 standard (see LWG issue 3234). These
overloads were not provided by MSVC STL until VS 2022 17.3.

These functions are unrelated to special member functions of class types.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_math_special_functions` | 201603L | (C++17) | Mathematical special
      functions

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3234 (P1467R9) | C++17 | additional overloads for math special functions
      were missing | these overloads are required

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):

---
*Source: https://en.cppreference.com/w/cpp/numeric/special_functions*
