# operator<<,>>(std::complex)

```cpp
template< class T, class CharT, class Traits >
std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os, const std::complex<T>& x );  // (1)
template< class T, class CharT, class Traits >
std::basic_istream<CharT, Traits>&
    operator>>( std::basic_istream<CharT, Traits>& is, std::complex<T>& x );  // (2)
```

1) Writes to `os` the complex number in the form `(real, imaginary)`.
2) Reads a complex number from `is`. The supported formats are

- `real`
- `(real)`
- `(real, imaginary)`

where the input for `real` and `imaginary` must be convertible to `T`. If an
error occurs calls `is.setstate(ios_base::failbit)`.

### Exceptions

May throw `std::ios_base::failure` on stream errors.

### Parameters

- **os** — a character output stream
- **is** — a character input stream
- **x** — the complex number to be inserted or extracted

### Return value

1) `os`

2) `is`

### Notes

1) As the comma may be used in the current locale as decimal separator, the
   output may be ambiguous. This can be solved with `std::showpoint` which
   forces the decimal separator to be visible.

2) The input is performed as a series of simple formatted extractions.
   Whitespace skipping is the same for each of them.

### Possible implementation

```cpp
template<class T, class CharT, class Traits>
basic_ostream<CharT, Traits>&
    operator<<(basic_ostream<CharT, Traits>& o, const complex<T>& x)
{
    basic_ostringstream<CharT, Traits> s;
    s.flags(o.flags());
    s.imbue(o.getloc());
    s.precision(o.precision());
    s << '(' << x.real() << ',' << x.imag() << ')';
    return o << s.str();
}
```

### Example

```cpp
#include <complex>
#include <iostream>

int main()
{
    std::cout << std::complex<double> {3.14, 2.71} << '\n';
}
```

Possible output:

```text
(3.14,2.71)
```

---
*Source: https://en.cppreference.com/w/cpp/numeric/complex/operator_ltltgtgt*
