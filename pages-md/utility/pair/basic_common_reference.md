# std::basic_common_reference<std::pair>

```cpp
template< class T1, class T2, class U1, class U2,
          template<class> class TQual, template<class> class UQual >
  requires requires { typename std::pair<std::common_reference_t<TQual<T1>, UQual<U1>>,
                                         std::common_reference_t<TQual<T2>, UQual<U2>>>; }
struct basic_common_reference<std::pair<T1, T2>, std::pair<U1, U2>, TQual, UQual>;  // (since C++23)
```

The common reference type of two `pair`s is a `pair` of both common reference
types of corresponding element types of both `pair`s, where the cv and reference
qualifiers on the `pair`s are applied to their element types.

The common reference type is defined only if both pairs of corresponding element
types have common reference types.

### Member types

- **`type`** — `std::pair<std::common_reference_t<TQual<T1>, UQual<U1>>,
  std::common_reference_t<TQual<T2>, UQual<U2>>>`

### Example

### See also

- **common_referencebasic_common_reference (C++20)** — determines the common
  reference type of a group of types (class template)
- **std::basic_common_reference<*tuple-like*> (C++23)** — determines the common
  reference type of a `tuple` and a `tuple-like` type (class template
  specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/pair/basic_common_reference*
