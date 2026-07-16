# std::indirect_unary_predicate

```cpp
template< class F, class I >
concept indirect_unary_predicate =
    std::indirectly_readable<I> &&
    std::copy_constructible<F> &&
    std::predicate<F&, std::iter_value_t<I>&> &&
    std::predicate<F&, std::iter_reference_t<I>> &&
    std::predicate<F&, std::iter_common_reference_t<I>>;  // (since C++20)
```

The concept `indirect_unary_predicate` specifies requirements for algorithms
that call unary predicates as their arguments. The key difference between this
concept and `std::predicate` is that it is applied to the type that `I`
references, rather than `I` itself.

### Semantic requirements

`F` and `I` model `indirect_unary_predicate` only if all concepts it subsumes
are modeled.

---
*Source: https://en.cppreference.com/w/cpp/iterator/indirect_unary_predicate*
