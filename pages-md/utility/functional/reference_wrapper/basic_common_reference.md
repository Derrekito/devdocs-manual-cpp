# std::basic_common_reference<std::reference_wrapper>

```cpp
template< class R, class T,
          template<class> RQual, template<class> TQual >
    requires (/*ref-wrap-common-reference-exists-with*/<R, T, RQual<R>, TQual<T>> &&
              !/*ref-wrap-common-reference-exists-with*/<T, R, TQual<T>, RQual<R>>)
struct basic_common_reference<R, T, RQual, TQual>;  // (1) (since C++23)
template< class T, class R,
          template<class> TQual, template<class> RQual >
    requires (/*ref-wrap-common-reference-exists-with*/<R, T, RQual<R>, TQual<T>> &&
              !/*ref-wrap-common-reference-exists-with*/<T, R, TQual<T>, RQual<R>>)
struct basic_common_reference<T, R, TQual, RQual>;  // (2) (since C++23)
Helper concepts
template< class R, class T, class RQ, class TQ >
concept /*ref-wrap-common-reference-exists-with*/ =
    /*is-ref-wrapper*/<R> &&
    requires { typename std::common_reference_t<typename R::type&, TQ>; } &&
    std::convertible<RQ, std::common_reference_t<typename R::type&, TQ>>;  // (3) (exposition only*)
```

The common reference type of a `reference_wrapper` (denoted as `R`) and a type
`T`, correspondingly applied with cv and reference qualifiers (denoted as `RQ`
and `TQ` respectively), is equivalent to a common reference type of underlying
type of `R` applied with lvalue reference and the `TQ`.

The common reference type is defined only if `R` is the only
`reference_wrapper`, the underlying type of `R` applied with lvalue reference
and the `TQ` have a common reference type to which `RQ` must be convertible.

The exposition-only constant `/*is-ref-wrapper*/<R>` is `true` if and only if
`R` is a specialization of `std::reference_wrapper`.

### Member types

- **`type`** — std::common_reference_t<typename R::type&, TQual<T>> (1,2)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_common_reference_wrapper` | 202302L | (C++23) | Make
      `std::common_reference_t` of `std::reference_wrapper` a reference type

### Example

```cpp
#include <concepts>
#include <functional>

static_assert(std::same_as<std::common_reference_t<int&,
                                                   std::reference_wrapper<int>>,
                                                   int&>);
static_assert(std::same_as<std::common_reference_t<std::reference_wrapper<int>&,
                                                   int&>,
                                                   int&>);
static_assert(std::same_as<std::common_reference_t<int&,
                                                   const std::reference_wrapper<int>&>,
                                                   int&>);
int main() {}
```

### See also

- **common_referencebasic_common_reference (C++20)** — determines the common
  reference type of a group of types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/reference_wrapper/basic_common_reference*
