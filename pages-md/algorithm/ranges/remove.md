# std::ranges::remove, std::ranges::remove_if

```cpp
Call signature
template< std::permutable I, std::sentinel_for<I> S, class T,
          class Proj = std::identity >
requires std::indirect_binary_predicate<ranges::equal_to,
             std::projected<I, Proj>, const T*>
constexpr ranges::subrange<I>
    remove( I first, S last, const T& value, Proj proj = {} );  // (1) (since C++20)
template< ranges::forward_range R, class T, class Proj = std::identity >
requires std::permutable<ranges::iterator_t<R>> &&
         std::indirect_binary_predicate<ranges::equal_to,
             std::projected<ranges::iterator_t<R>, Proj>, const T*>
constexpr ranges::borrowed_subrange_t<R>
    remove( R&& r, const T& value, Proj proj = {} );  // (2) (since C++20)
template< std::permutable I, std::sentinel_for<I> S, class Proj = std::identity,
          std::indirect_unary_predicate<std::projected<I, Proj>> Pred >
constexpr ranges::subrange<I>
    remove_if( I first, S last, Pred pred, Proj proj = {} );  // (3) (since C++20)
template< ranges::forward_range R, class Proj = std::identity,
          std::indirect_unary_predicate<
              std::projected<ranges::iterator_t<R>, Proj>> Pred >
requires std::permutable<ranges::iterator_t<R>>
constexpr ranges::borrowed_subrange_t<R>
    remove_if( R&& r, Pred pred, Proj proj = {} );  // (4) (since C++20)
```

Removes all elements satisfying specific criteria from the range
`[``first``,``last``)` and returns a subrange `[``ret``,``last``)`, where `ret`
is a past-the-end iterator for the new end of the range.

1) Removes all elements that are equal to `value`, using `std::invoke(proj, *i)
   == value` to compare.

3) Removes all elements for which `std::invoke(pred, std::invoke(proj, *i))`
   returns `true`.

2,4) Same as (1,3), but uses `r` as the range, as if using `ranges::begin(r)` as
   `first` and `ranges::end(r)` as `last`.

Removing is done by shifting (by means of move assignment) the elements in the
range in such a way that the elements that are not to be removed appear in the
beginning of the range. Relative order of the elements that remain is preserved
and the *physical* size of the container is unchanged. Iterators pointing to an
element between the new *logical* end and the *physical* end of the range are
still dereferenceable, but the elements themselves have unspecified values (as
per MoveAssignable post-condition).

The function-like entities described on this page are *niebloids*, that is:

- Explicit template argument lists cannot be specified when calling any of them.
- None of them are visible to argument-dependent lookup.
- When any of them are found by normal unqualified lookup as the name to the
  left of the function-call operator, argument-dependent lookup is inhibited.

In practice, they may be implemented as function objects, or with special
compiler extensions.

### Parameters

- **first, last** — the range of elements to process
- **r** — the range of elements to process
- **value** — the value of elements to remove
- **pred** — predicate to apply to the projected elements
- **proj** — projection to apply to the elements

### Return value

`{ret, last}`, where `[``first``,``ret``)` is the resulting subrange after
removal, and the elements in subrange `[``ret``,``last``)` are all in valid but
unspecified state.

### Complexity

Exactly `N` applications of the corresponding predicate and any projection,
where `N = ranges::distance(first, last)`, and `N - 1` move operations at worst.

### Notes

A call to `ranges::remove` is typically followed by a call to a container's
`erase` member function, which erases the unspecified values and reduces the
*physical* size of the container to match its new *logical* size. These two
invocations together constitute a so-called Erase-remove idiom, which can be
achieved by the free function `std::erase` that has overloads for all standard
*sequence* containers, or `std::erase_if` that has overloads for *all* standard
containers.

The similarly-named container member functions `list::remove`,
`list::remove_if`, `forward_list::remove`, and `forward_list::remove_if` erase
the removed elements.

These algorithms usually cannot be used with associative containers such as
`std::set` and `std::map` because their iterator types do not dereference to
MoveAssignable types (the keys in these containers are not modifiable).

Because `ranges::remove` takes `value` by reference, it can have unexpected
behavior if it is a reference to an element of the range `[``first``,``last``)`.

### Possible implementation

```cpp
struct remove_fn
{
    template<std::permutable I, std::sentinel_for<I> S, class T,
             class Proj = std::identity>
    requires std::indirect_binary_predicate<
                 ranges::equal_to, std::projected<I, Proj>, const T*>
    constexpr ranges::subrange<I>
        operator()(I first, S last, const T& value, Proj proj = {}) const
    {
        first = ranges::find(std::move(first), last, value, proj);
        if (first != last)
        {
            for (I i{std::next(first)}; i != last; ++i)
                if (value != std::invoke(proj, *i))
                {
                    *first = ranges::iter_move(i);
                    ++first;
                }
        }
        return {first, last};
    }

    template<ranges::forward_range R, class T, class Proj = std::identity>
    requires std::permutable<ranges::iterator_t<R>> &&
             std::indirect_binary_predicate<
                 ranges::equal_to, std::projected<
                     ranges::iterator_t<R>, Proj>, const T*>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, const T& value, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), value, std::move(proj));
    }
};

inline constexpr remove_fn remove {};
```

```cpp
struct remove_if_fn
{
    template<std::permutable I, std::sentinel_for<I> S, class Proj = std::identity,
             std::indirect_unary_predicate<std::projected<I, Proj>> Pred>
    constexpr ranges::subrange<I>
        operator()(I first, S last, Pred pred, Proj proj = {}) const
    {
        first = ranges::find_if(std::move(first), last, pred, proj);
        if (first != last)
        {
            for (I i{std::next(first)}; i != last; ++i)
                if (!std::invoke(pred, std::invoke(proj, *i)))
                {
                    *first = ranges::iter_move(i);
                    ++first;
                }
        }
        return {first, last};
    }

    template<ranges::forward_range R, class Proj = std::identity,
             std::indirect_unary_predicate<
                 std::projected<ranges::iterator_t<R>, Proj>> Pred>
    requires std::permutable<ranges::iterator_t<R>>
    constexpr ranges::borrowed_subrange_t<R>
        operator()(R&& r, Pred pred, Proj proj = {}) const
    {
        return (*this)(ranges::begin(r), ranges::end(r), pred, std::move(proj));
    }
};

inline constexpr remove_if_fn remove_if {};
```

### Example

```cpp
#include <algorithm>
#include <cctype>
#include <iomanip>
#include <iostream>
#include <string>
#include <string_view>

int main()
{
    std::string v1{"No - Diagnostic - Required"};
    std::cout << std::quoted(v1) << " (v1, size: " << v1.size() << ")\n";
    const auto ret = std::ranges::remove(v1, ' ');
    std::cout << std::quoted(v1) << " (v1 after `remove`, size: " << v1.size() << ")\n";
    std::cout << ' ' << std::string(std::distance(v1.begin(), ret.begin()), '^') << '\n';
    v1.erase(ret.begin(), ret.end());
    std::cout << std::quoted(v1) << " (v1 after `erase`, size: " << v1.size() << ")\n\n";

    // remove_if with custom unary predicate:
    auto rm = [](char c) { return !std::isupper(c); };
    std::string v2{"Substitution Failure Is Not An Error"};
    std::cout << std::quoted(v2) << " (v2, size: " << v2.size() << ")\n";
    const auto [first, last] = std::ranges::remove_if(v2, rm);
    std::cout << std::quoted(v2) << " (v2 after `remove_if`, size: " << v2.size() << ")\n";
    std::cout << ' ' << std::string(std::distance(v2.begin(), first), '^') << '\n';
    v2.erase(first, last);
    std::cout << std::quoted(v2) << " (v2 after `erase`, size: " << v2.size() << ")\n\n";

    // creating a view into a container that is modified by `remove_if`:
    for (std::string s : {"Small Object Optimization", "Non-Type Template Parameter"})
        std::cout << std::quoted(s) << " => "
            << std::string_view{begin(s), std::ranges::remove_if(s, rm).begin()} << '\n';
}
```

Possible output:

```text
"No _ Diagnostic _ Required" (v1, size: 26)
"No_Diagnostic_Requiredired" (v1 after `remove`, size: 26)
 ^^^^^^^^^^^^^^^^^^^^^^
"No_Diagnostic_Required" (v1 after `erase`, size: 22)

"Substitution Failure Is Not An Error" (v2, size: 36)
"SFINAEtution Failure Is Not An Error" (v2 after `remove_if`, size: 36)
 ^^^^^^
"SFINAE" (v2 after `erase`, size: 6)

"Small Object Optimization" => SOO
"Non-Type Template Parameter" => NTTP
```

### See also

- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **ranges::unique (C++20)** — removes consecutive duplicate elements in a range
  (niebloid)
- **removeremove_if** — removes elements satisfying specific criteria (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/remove*
