# Standard library header <limits>

This header is part of the type support library.

### Declarations

- **numeric_limits** — provides an interface to query properties of all
  fundamental numeric types (class template)
- **float_round_style** — indicates floating-point rounding modes (enum)
- **float_denorm_style** — indicates floating-point denormalization modes (enum)

### Synopsis

```cpp
namespace std {
    enum float_round_style;
    enum float_denorm_style;

    template<class T> class numeric_limits;

    template<class T> class numeric_limits<const T>;
    template<class T> class numeric_limits<volatile T>;
    template<class T> class numeric_limits<const volatile T>;

    template<> class numeric_limits<bool>;

    template<> class numeric_limits<char>;
    template<> class numeric_limits<signed char>;
    template<> class numeric_limits<unsigned char>;
    template<> class numeric_limits<char8_t>;
    template<> class numeric_limits<char16_t>;
    template<> class numeric_limits<char32_t>;
    template<> class numeric_limits<wchar_t>;

    template<> class numeric_limits<short>;
    template<> class numeric_limits<int>;
    template<> class numeric_limits<long>;
    template<> class numeric_limits<long long>;
    template<> class numeric_limits<unsigned short>;
    template<> class numeric_limits<unsigned int>;
    template<> class numeric_limits<unsigned long>;
    template<> class numeric_limits<unsigned long long>;

    template<> class numeric_limits<float>;
    template<> class numeric_limits<double>;
    template<> class numeric_limits<long double>;
}
```

#### Enumeration `std::float_round_style`

```cpp
namespace std {
    enum float_round_style {
        round_indeterminate       = -1,
        round_toward_zero         =  0,
        round_to_nearest          =  1,
        round_toward_infinity     =  2,
        round_toward_neg_infinity =  3,
    };
}
```

#### Enumeration `std::float_denorm_style`

```cpp
namespace std {
    enum float_denorm_style {
        denorm_indeterminate = -1,
        denorm_absent        =  0,
        denorm_present       =  1
    };
}
```

#### Class template `std::numeric_limits`

```cpp
template<class T> class numeric_limits {
public:
    static constexpr bool is_specialized = false;

    static constexpr T min() noexcept { return T(); }
    static constexpr T max() noexcept { return T(); }
    static constexpr T lowest() noexcept { return T(); }

    static constexpr int digits = 0;
    static constexpr int digits10 = 0;
    static constexpr int max_digits10 = 0;

    static constexpr bool is_signed = false;
    static constexpr bool is_integer = false;
    static constexpr bool is_exact = false;
    static constexpr int radix = 0;
    static constexpr T epsilon() noexcept { return T(); }
    static constexpr T round_error() noexcept { return T(); }

    static constexpr int min_exponent = 0;
    static constexpr int min_exponent10 = 0;
    static constexpr int max_exponent = 0;
    static constexpr int max_exponent10 = 0;

    static constexpr bool has_infinity = false;
    static constexpr bool has_quiet_NaN = false;
    static constexpr bool has_signaling_NaN = false;
    static constexpr float_denorm_style has_denorm = denorm_absent;
    static constexpr bool has_denorm_loss = false;
    static constexpr T infinity() noexcept { return T(); }
    static constexpr T quiet_NaN() noexcept { return T(); }
    static constexpr T signaling_NaN() noexcept { return T(); }
    static constexpr T denorm_min() noexcept { return T(); }

    static constexpr bool is_iec559 = false;
    static constexpr bool is_bounded = false;
    static constexpr bool is_modulo = false;

    static constexpr bool traps = false;
    static constexpr bool tinyness_before = false;
    static constexpr float_round_style round_style = round_toward_zero;
};
```

#### Specialization std::numeric_limits<bool>

```cpp
template<> class numeric_limits<bool> {
public:
    static constexpr bool is_specialized = true;

    static constexpr bool min() noexcept { return false; }
    static constexpr bool max() noexcept { return true; }
    static constexpr bool lowest() noexcept { return false; }

    static constexpr int digits = 1;
    static constexpr int digits10 = 0;
    static constexpr int max_digits10 = 0;

    static constexpr bool is_signed = false;
    static constexpr bool is_integer = true;
    static constexpr bool is_exact = true;
    static constexpr int radix = 2;
    static constexpr bool epsilon() noexcept { return 0; }
    static constexpr bool round_error() noexcept { return 0; }

    static constexpr int min_exponent = 0;
    static constexpr int min_exponent10 = 0;
    static constexpr int max_exponent = 0;
    static constexpr int max_exponent10 = 0;

    static constexpr bool has_infinity = false;
    static constexpr bool has_quiet_NaN = false;
    static constexpr bool has_signaling_NaN = false;
    static constexpr float_denorm_style has_denorm = denorm_absent;
    static constexpr bool has_denorm_loss = false;
    static constexpr bool infinity() noexcept { return 0; }
    static constexpr bool quiet_NaN() noexcept { return 0; }
    static constexpr bool signaling_NaN() noexcept { return 0; }
    static constexpr bool denorm_min() noexcept { return 0; }

    static constexpr bool is_iec559 = false;
    static constexpr bool is_bounded = true;
    static constexpr bool is_modulo = false;

    static constexpr bool traps = false;
    static constexpr bool tinyness_before = false;
    static constexpr float_round_style round_style = round_toward_zero;
};
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 184 | C++98 | the definition of the specialization
      std::numeric_limits<bool> was not provided | provided
  LWG 559 | C++98 | the `std::numeric_limits` specializations of cv-qualified
      arithmetic types were missing in the synopsis | added

---
*Source: https://en.cppreference.com/w/cpp/header/limits*
