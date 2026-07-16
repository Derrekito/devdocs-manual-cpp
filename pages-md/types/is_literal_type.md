# std::is_literal_type

```cpp
template< class T >
struct is_literal_type;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

`std::is_literal_type` is a UnaryTypeTrait.

(This type trait has been deprecated[1] and removed[2] as offering negligible
value to generic code.)

If `T` satisfies all requirements of LiteralType, provides the member constant
`value` equal to `true`. For any other type, `value` is `false`.

If std::remove_all_extents_t<T> is an incomplete type and not (possibly
cv-qualified) void, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_literal_type`
or `std::is_literal_type_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_literal_type_v = is_literal_type<T>::value;  // (since C++17) (deprecated) (removed in C++20)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a literal type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Only literal types may be used as parameters to or returned from constexpr
functions. Only literal classes may have constexpr member functions.

### Example

```cpp
#include <type_traits>

struct A { int m; };
static_assert(std::is_literal_type_v<A> == true);

struct B { virtual ~B(); };
static_assert(std::is_literal_type_v<B> == false);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | `T` could be an array of incomplete class type with unknown
      bound | the behavior is undefined in this case

### External links

  Alisdair Meredith. "Deprecate the `is_literal` Trait". *Deprecating Vestigial
      Library Parts in C++17*. "The `is_literal` type trait offers negligible
      value to generic code, as what is really needed is the ability to know
      that a specific construction would produce constant initialization."
      Alisdair Meredith, Stephan T. Lavavej, Tomasz Kamiński. "Deprecated type
      traits". *Reviewing Deprecated Facilities of C++17 for C++20*. "**Strong
      recommendation:** Remove the traits that can live on as zombies. [...]
      **Toronto Review:** Accept strong recommendation, strike from C++20."

---
*Source: https://en.cppreference.com/w/cpp/types/is_literal_type*
