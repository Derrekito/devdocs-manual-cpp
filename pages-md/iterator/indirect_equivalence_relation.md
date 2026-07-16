# std::indirect_equivalence_relation

```cpp
template< class F, class I1, class I2 = I1 >
concept indirect_equivalence_relation =
    std::indirectly_readable<I1> &&
    std::indirectly_readable<I2> &&
    std::copy_constructible<F> &&
    std::equivalence_relation<F&, std::iter_value_t<I1>&, std::iter_value_t<I2>&> &&
    std::equivalence_relation<F&, std::iter_value_t<I1>&, std::iter_reference_t<I2>> &&
    std::equivalence_relation<F&, std::iter_reference_t<I1>, std::iter_value_t<I2>&> &&
    std::equivalence_relation<F&, std::iter_reference_t<I1>, std::iter_reference_t<I2>> &&
    std::equivalence_relation<F&, std::iter_common_reference_t<I1>,
                                  std::iter_common_reference_t<I2>>;  // (since C++20)
```

The concept `indirect_equivalence_relation` specifies requirements for
algorithms that call equivalence relations as their arguments. The key
difference between this concept and `std::equivalence_relation` is that it is
applied to the types that `I1` and `I2` references, rather than `I1` and `I2`
themselves.

### Semantic requirements

`F`, `I1`, and `I2` model `indirect_equivalence_relation` only if all concepts
it subsumes are modeled.

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirect_equivalence_relation*
