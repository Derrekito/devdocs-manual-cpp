# std::iterator_traits<std::counted_iterator>

```cpp
template< std::input_iterator I >
    requires /* see below */
struct iterator_traits<std::counted_iterator<I>> : std::iterator_traits<I> {
    using pointer = std::conditional_t<std::contiguous_iterator<I>,
                                       std::add_pointer_t<std::iter_reference_t<I>>,
                                       void>;
};  // (since C++20)
```

Inherits the properties from customized (generated from either a standard
partial specialization or a program-defined specialization)
`std::iterator_traits<I>`, with the member type `pointer` adjusted, where `I`
models `input_iterator`.

Notably, the `iterator_concept` (if present) and `iterator_category` are
inherited from `std::iterator_traits<I>`.

The condition in the requires-clause is `true` if and only if
`std::iterator_traits<I>` is not generated from the primary template.

### Note

Before P2259R1, this specialization is used even if `std::iterator_traits<I>` is
generated from the primary template. As a result, when testing
`std::counted_iterator<I>` against an iterator concept (e.g.
`forward_iterator`), the determination of `/*ITER_CONCEPT*/` does not take
`I::iterator_concept` into account, and thus `std::counted_iterator<I>`
sometimes erroneously behaves as if it cannot model that concept. This incorrect
behavior is implemented in libstdc++ prior to 10.4, and in MSVC STL prior to VS
2022 17.0 Preview 3.

The standard library provides partial specializations of `std::iterator_traits`
for pointer types, `std::counted_iterator`, and `std::common_iterator`.

### Example

```cpp
#include <iterator>
#include <list>
#include <type_traits>
#include <vector>

int main()
{
    std::vector v{1, 2, 3, 4};
    std::list l{1, 2, 3, 4};
    std::counted_iterator iv{v.begin(), 3};
    std::counted_iterator il{l.begin(), 3};
    static_assert(std::is_same<int*, std::iterator_traits<decltype(iv)>::pointer>());
    static_assert(std::is_same<void, std::iterator_traits<decltype(il)>::pointer>());
}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | there's no requires-clause `pointer` is unconditionally
      defined as `void` | constraint added

### See also

- **iterator_traits** — provides uniform interface to the properties of an
  iterator (class template)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/iterator_traits*
