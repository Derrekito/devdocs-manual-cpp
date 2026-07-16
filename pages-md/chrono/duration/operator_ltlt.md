# std::chrono::operator<< (std::chrono::duration)

```cpp
template<
    class CharT,
    class Traits,
    class Rep,
    class Period
> std::basic_ostream<CharT, Traits>&
    operator<<( std::basic_ostream<CharT, Traits>& os,
                const std::chrono::duration<Rep, Period>& d );  // (since C++20)
```

Inserts a textual representation of `d` into `os`.

Behaves as if it was implemented as

```cpp
std::basic_ostringstream<CharT, Traits> s;
s.flags(os.flags());
s.imbue(os.getloc());
s.precision(os.precision());
s << d.count() << units_suffix; // see below
return os << s.str();
```

In other words, the stream flags, locale, and precision are determined by the
stream, but any padding are determined using the entire output string.

The `units_suffix` is determined based on `Period::type` according to the
following table.

- **`std::atto`** — `as`
- **`std::femto`** — `fs`
- **`std::pico`** — `ps`
- **`std::nano`** — `ns`
- **`std::micro`** — `µs` (U+00B5) or `us`, it is implementation-defined which
  one is used
- **`std::milli`** — `ms`
- **`std::centi`** — `cs`
- **`std::deci`** — `ds`
- **`std::ratio<1>`** — `s`
- **`std::deca`** — `das`
- **`std::hecto`** — `hs`
- **`std::kilo`** — `ks`
- **`std::mega`** — `Ms`
- **`std::giga`** — `Gs`
- **`std::tera`** — `Ts`
- **`std::peta`** — `Ps`
- **`std::exa`** — `Es`
- **`std::ratio<60>`** — `min`
- **`std::ratio<3600>`** — `h`
- **`std::ratio<86400>`** — `d`
- **None of the above, and `Period::type::den == 1`** — `[num]s`
- **None of the above** — `[num/den]s`

For the last two rows of the table, `num` and `den` in the suffix are
`Period::type::num` and `Period::type::den` formatted as a decimal number with
no leading zeroes, respectively.

### Return value

A reference to the stream, i.e., `os`.

### Example

This example shows the output of `std::chrono::operator<<` when given a
duration:

```cpp
#include <chrono>
#include <iostream>
using namespace std::chrono_literals;

int main()
{
    constexpr auto duration = 123ms;
    std::cout << duration << '\n';
}
```

Output:

```text
123ms
```

### See also

- **format (C++20)** — stores formatted representation of the arguments in a new
  string (function template)
- **std::formatter<std::chrono::duration> (C++20)** — formatting support for
  `duration` (class template specialization)
- **operator<<operator>>** — performs stream input and output on strings
  (function template)
- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)
- **to_wstring (C++11)** — converts an integral or floating-point value to
  `wstring` (function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration/operator_ltlt*
