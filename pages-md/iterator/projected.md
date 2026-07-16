# std::projected

```cpp
template< std::indirectly_readable I,
          std::indirectly_regular_unary_invocable<I> Proj >
struct projected {
    using value_type = std::remove_cvref_t<std::indirect_result_t<Proj&, I>>;
    std::indirect_result_t<Proj&, I> operator*() const; // not defined
};  // (since C++20) (until C++26)
template< std::indirectly_readable I,
          std::indirectly_regular_unary_invocable<I> Proj >
using projected = /*projected-impl*/<I, Proj>::/*__type*/; // see below  // (since C++26)
template< std::weakly_incrementable I, class Proj >
struct incrementable_traits<std::projected<I, Proj>> {
    using difference_type = std::iter_difference_t<I>;
};  // (2) (since C++20) (until C++26)
template< class I, class Proj >
struct /*projected-impl*/ { // exposition only
    struct /*__type*/ { // exposition only
        using value_type = std::remove_cvref_t<std::indirect_result_t<Proj&, I>>;
        using difference_type = std::iter_difference_t<I>; // conditionally present

        std::indirect_result_t<Proj&, I> operator*() const; // not defined
    };
};  // (3) (since C++26)
```

1) Class(until C++26)Alias(since C++26) template `projected` combines an
   `indirectly_readable` type `I` and a callable object type `Proj` into a new
   `indirectly_readable` type whose reference type is the result of applying
   `Proj` to the `std::iter_reference_t<I>`.

2) This specialization of `std::incrementable_traits` makes `std::projected<I,
   Proj>` a `weakly_incrementable` type when `I` is also a
   `weakly_incrementable` type.

3) An indirect layer used for avoiding unexpected argument-dependent lookup. The
   member type `difference_type` exists only if `I` models
   `weakly_incrementable`.

`projected` is used only to constrain algorithms that accept callable objects
and projections, and hence its `operator*()` is not defined.

### Template parameters

- **I** — an indirectly readable type
- **Proj** — projection applied to a dereferenced `I`

### Notes

The indirect layer prevents `I` and `Proj` to be associated classes of
`projected`. When an associated class of `I` or `Proj` is an incomplete class
type, the indirect layer avoids the unnecessary attempt to inspect the
definition of that type that results in hard error.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <functional>
#include <iterator>

template<class T>
struct Holder
{
    T t;
};

struct Incomplete;

using P = Holder<Incomplete>*;

static_assert(std::equality_comparable<P>); // OK
static_assert(std::indirectly_comparable<P*, P*, std::equal_to<>>); // Error before C++26
static_assert(std::sortable<P*>); // Error before C++26

int main()
{
    P a[10] = {}; // ten null pointers
    assert(std::count(a, a + 10, nullptr) == 10); // OK
    assert(std::ranges::count(a, a + 10, nullptr) == 10); // Error before C++26
}
```

---
*Source: https://en.cppreference.com/w/cpp/iterator/projected*
