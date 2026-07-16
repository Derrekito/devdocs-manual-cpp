# std::literals::complex_literals::operator""i, operator""if, operator""il

```cpp
constexpr complex<double> operator""i( long double arg );
constexpr complex<double> operator""i( unsigned long long arg );  // (1) (since C++14)
constexpr complex<float> operator""if( long double arg );
constexpr complex<float> operator""if( unsigned long long arg );  // (2) (since C++14)
constexpr complex<long double> operator""il( long double arg );
constexpr complex<long double> operator""il( unsigned long long arg );  // (3) (since C++14)
```

Forms a `std::complex` literal representing an imaginary number.

1) Forms a literal `std::complex<double>` with the real part zero and imaginary
   part `arg`.

2) Forms a literal `std::complex<float>` with the real part zero and imaginary
   part `arg`.

3) Forms a literal `std::complex<long double>` with the real part zero and
   imaginary part `arg`.

### Parameters

- **arg** — the value of the imaginary number

### Return value

The `std::complex` literal with the real part zero and imaginary part `arg`.

### Notes

These operators are declared in the namespace `std::literals::complex_literals`,
where both `literals` and `complex_literals` are inline namespaces. Access to
these operators can be gained with either:

- `using namespace std::literals`,
- `using namespace std::complex_literals`, or
- `using namespace std::literals::complex_literals`.

Even though `if` is a keyword in C++, it is a ud-suffix of the literal operator
of the form `operator ""if` and in the literal expressions such as `1if` or
`1.0if` because it is not separated by whitespace and is not a standalone token.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_complex_udls` | 201309L | (C++14) | User-Defined Literals for
      `std::complex`

### Possible implementation

```cpp
constexpr std::complex<double> operator""i(unsigned long long d)
{
    return std::complex<double> {0.0, static_cast<double>(d)};
}

constexpr std::complex<double> operator""i(long double d)
{
    return std::complex<double> {0.0, static_cast<double>(d)};
}
```

```cpp
constexpr std::complex<float> operator""if(unsigned long long d)
{
    return std::complex<float> {0.0f, static_cast<float>(d)};
}

constexpr std::complex<float> operator""if(long double d)
{
    return std::complex<float> {0.0f, static_cast<float>(d)};
}
```

```cpp
constexpr std::complex<long double> operator""il(unsigned long long d)
{
    return std::complex<long double> {0.0L, static_cast<long double>(d)};
}

constexpr std::complex<long double> operator""il(long double d)
{
    return std::complex<long double> {0.0L, d};
}
```

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    using namespace std::complex_literals;

    std::complex<double> c = 1.0 + 1i;
    std::cout << "abs" << c << " = " << std::abs(c) << '\n';

    std::complex<float> z = 3.0f + 4.0if;
    std::cout << "abs" << z << " = " << std::abs(z) << '\n';
}
```

Output:

```text
abs(1,1) = 1.41421
abs(3,4) = 5
```

### See also

- **(constructor)** — constructs a complex number (public member function)
- **operator=** — assigns the contents (public member function)

**C documentation for `I`**

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/operator%22%22i*
