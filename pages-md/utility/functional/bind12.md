# std::bind1st, std::bind2nd

```cpp
template< class F, class T >
std::binder1st<F> bind1st( const F& f, const T& x );  // (1) (deprecated in C++11) (removed in C++17)
template< class F, class T >
std::binder2nd<F> bind2nd( const F& f, const T& x );  // (2) (deprecated in C++11) (removed in C++17)
```

Binds a given argument `x` to a first or second parameter of the given binary
function object `f`. That is, stores `x` within the resulting wrapper, which, if
called, passes `x` as the first or the second parameter of `f`.

1) Binds the first argument of `f` to `x`. Effectively calls
   `std::binder1st<F>(f, typename F::first_argument_type(x))`.

2) Binds the second argument of `f` to `x`. Effectively calls
   `std::binder2nd<F>(f, typename F::second_argument_type(x))`.

### Parameters

- **f** — pointer to a function to bind an argument to
- **x** — argument to bind to `f`

### Return value

A function object wrapping `f` and `x`.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <algorithm>
#include <cmath>
#include <cstddef>
#include <functional>
#include <iomanip>
#include <iostream>
#include <vector>

int main()
{
    std::vector<double> a = {0, 30, 45, 60, 90, 180};
    std::vector<double> r(a.size());
    const double pi = std::acos(-1); // since C++20 use std::numbers::pi

    std::transform(a.begin(), a.end(), r.begin(),
        std::bind1st(std::multiplies<double>(), pi / 180.0));
//  an equivalent lambda is: [pi](double a) { return a * pi / 180.0; });

    for (std::size_t n = 0; n < a.size(); ++n)
        std::cout << std::setw(3) << a[n] << "° = " << std::fixed << r[n]
                  << " rad\n" << std::defaultfloat;
}
```

Output:

```text
  0° = 0.000000 rad
 30° = 0.523599 rad
 45° = 0.785398 rad
 60° = 1.047198 rad
 90° = 1.570796 rad
180° = 3.141593 rad
```

### See also

- **binder1stbinder2nd (deprecated in C++11)(removed in C++17)** — function
  object holding a binary function and one of its arguments (class template)
- **bind_frontbind_back (C++20)(C++23)** — bind a variable number of arguments,
  in order, to a function object (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/bind12*
