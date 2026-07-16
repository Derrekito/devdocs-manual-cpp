# std::ranges::to

```cpp
template< class C, ranges::input_range R, class... Args >
    requires (!ranges::view<C>)
constexpr C to( R&& r, Args&&... args );  // (1) (since C++23)
template< template< class... > class C, ranges::input_range R, class... Args >
constexpr auto to( R&& r, Args&&... args );  // (2) (since C++23)
template< class C, class... Args >
    requires (!ranges::view<C>)
constexpr /*range adaptor closure*/ to( Args&&... args );  // (3) (since C++23)
template< template< class... > class C, class... Args >
constexpr /*range adaptor closure*/ to( Args&&... args );  // (4) (since C++23)
Helper templates
template< class Container >
constexpr bool /*reservable-container*/ =
    ranges::sized_range<Container> &&
    requires (Container& c, ranges::range_size_t<Container> n)
    {
        c.reserve(n);
        { c.capacity() } -> std::same_as<decltype(n)>;
        { c.max_size() } -> std::same_as<decltype(n)>;
    };  // (5) (exposition only*)
template< class Container, class Reference >
constexpr bool /*container-insertable*/ = requires (Container& c, Reference&& ref)
{
    requires (requires { c.push_back(std::forward<Reference>(ref)); } ||
              requires { c.insert(c.end(), std::forward<Reference>(ref)); });
};  // (6) (exposition only*)
template< class Reference, class C >
constexpr auto /*container-inserter*/( C& c )
{
    if constexpr (requires { c.push_back(std::declval<Reference>()); })
        return std::back_inserter(c);
    else
        return std::inserter(c, c.end());
}  // (7) (exposition only*)
template< class R, class T >
concept /*container-compatible-range*/ =
    ranges::input_range<R> &&
    std::convertible_to<ranges::range_reference_t<R>, T>;  // (8) (exposition only*)
```

The overloads of the range conversion function construct a new non-view object
from a source range as its first argument by calling a constructor taking a
range, a `std::from_range_t` tagged ranged constructor, a constructor taking an
iterator-sentinel pair, or by back inserting each element of the source range
into the arguments-constructed object.

1) Constructs an object of type `C` from the elements of `r` in the following:

   a) If `C` does not satisfy `input_range` or
      `std::convertible_to<ranges::range_reference_t<R>,
      ranges::range_value_t<C>>` is `true`:

1) Constructing a non-view object as if direct-initializing (but not
direct-list-initializing) an object of type `C` from the source range
`std::forward<R>(r)` and the rest of the functional arguments
`std::forward<Args>(args)...` if `std::constructible_from<C, R, Args...>` is
`true`.

2) Otherwise, constructing a non-view object as if direct-initializing (but not
direct-list-initializing) an object of type `C` from additional disambiguation
tag `std::from_range`, the source range `std::forward<R>(r)` and the rest of the
functional arguments `std::forward<Args>(args)...` if
`std::constructible_from<C, std::from_range_t, R, Args...>` is `true`.

3) Otherwise, constructing a non-view object as if direct-initializing (but not
direct-list-initializing) an object of type `C` from the iterator-sentinel pair
(`ranges::begin(r)` as an iterator and `ranges::end(r)` as sentinel, where
iterator and sentinel have the same type. In other words, the source range must
be a common range), and the rest of function arguments
`std::forward<Args>(args)...` if all of the conditions below are `true`:

- `ranges::common_range<R>`
- If `std::iterator_traits<ranges::iterator_t<R>>::iterator_category` is valid
  and denotes a type that satisfies `std::derived_from<std::input_iterator_tag>`
- `std::constructible_from<C, ranges::iterator_t<R>, ranges::sentinel_t<R>,
  Args...>`

4) Otherwise, constructing a non-view range object as if direct-initializing
(but not direct-list-initializing) an object of type `C` from the rest of the
function arguments `std::forward<Args>(args)...` with the following equivalent
call below after the construction:

`if constexpr (ranges::sized_range<R> && /*reservable-container*/<C>)
c.reserve(static_cast<ranges::range_size_t<C>>(ranges::size(r)));
ranges::copy(r, /*container-inserter*/<ranges::range_reference_t<R>>(c));`
If the `R` satisfies `sized_range` and `C` satisfies `/*reservable-container*/`,
the constructed object `c` of type `C` is able to reserve storage with the
initial storage size `ranges::size(r)` to prevent additional allocations during
inserting new elements. Each range reference element of `r` is back inserted to
`c` through `ranges::copy` with back inserter adaptor. The operations above are
valid if both of the conditions below are `true`:

- `std::constructible_from<C, Args...>`
- `/*container-insertable*/<C, ranges::range_reference_t<R>>`

   b) Otherwise, the return expression is equivalent to: `to<C>(r |
      views::transform([](auto&& elem){ return
      to<ranges::range_value_t<C>>(std::forward<decltype(elem)>(elem));}),
      std::forward<Args>(args)...)` Which allows nested range constructions
      within the range if `ranges::input_range<ranges::range_reference_t<C>>` is
      `true`. Otherwise, the program is ill-formed.
2) Constructs an object of deduced type from the elements of `r`.

Let `/*input-iterator*/` be an exposition only type that satisfies
LegacyInputIterator:

```cpp
struct /*input-iterator*/
{
    using iterator_category = std::input_iterator_tag;
    using value_type = ranges::range_value_t<R>;
    using difference_type = std::ptrdiff_t;
    using pointer = std::add_pointer_t<ranges::range_reference_t<R>>;
    using reference = ranges::range_reference_t<R>;
    reference operator*() const;                      // not defined
    pointer operator->() const;                       // not defined
    /*input-iterator*/& operator++();                 // not defined
    /*input-iterator*/ operator++(int);               // not defined
    bool operator==(const /*input-iterator*/&) const; // not defined
};  // (exposition only*)
```

Let `/*DEDUCE-EXPR*/` be defined as follows:

- `C(std::declval<R>(), std::declval<Args>()...)`, if that expression is valid.
- Otherwise, `C(std::from_range, std::declval<R>(), std::declval<Args>()...)`,
  if that expression is valid.
- Otherwise, `C(std::declval</*input-iterator*/>(),
  std::declval</*input-iterator*/>(), std::declval<Args>()...)`, if that
  expression is valid.
- Otherwise, the program is ill-formed.

The call is equivalent to `to<decltype(/*DEDUCE-EXPR*/)>(std::forward<R>(r),
std::forward<Args>(args)...)`.

3,4) Returns a perfect forwarding call wrapper that is also a
   RangeAdaptorClosureObject.

5) The exposition-only variable template `/*reservable-container*/<Container>`
   is `true` if it satisfies `ranges::sized_range` and is eligible to be
   reservable.

6) The exposition-only variable template `/*container-insertable*/<Container,
   Reference>` is `true` if `Container` is back insertable by a member function
   call `push_back` or `insert`.

7) The exposition-only function template `/*container-inserter*/` returns an
   output iterator of type `std::back_insert_iterator` if member function
   `push_back` is available, otherwise the type is `std::insert_iterator`.

8) The exposition-only concept `/*container-compatible-range*/` is used in the
   definition of containers in constructing an input range `R` and its range
   reference type must be convertible to `T`.

### Parameters

- **r** — a source range object
- **args** — list of the arguments to (1,2) construct a range or (3,4) bind to
  the last parameters of range adaptor closure object

### Return value

1,2) A constructed non-view object.

3,4) A range adaptor closure object of unspecified type, with the following
   properties: ranges::to return type The return type is derived from
   `ranges::range_adaptor_closure</*return-type*/>`. Member objects The returned
   object behaves as if it has no target object, and an `std::tuple` object
   `tup` constructed with
   `std::tuple<std::decay_t<Args>...>(std::forward<Args>(args)...)`, except that
   the returned object's assignment behavior is unspecified and the names are
   for exposition only. Constructors The return type of `ranges::to` (3-4)
   behaves as if its copy/move constructors perform a memberwise copy/move. It
   is CopyConstructible if all of its member objects (specified above) are
   CopyConstructible, and is MoveConstructible otherwise. Member function
   `operator()` Given an object `G` obtained from an earlier call to
   `range::to</* see below */>(args...)`, when a glvalue `g` designating `G` is
   invoked in a function call expression `g(r)`, an invocation of the stored
   object takes place, as if by `ranges::to</* see below */>(r,
   std::get<Ns>(g.tup)...)`, where `r` is a source range object that must
   satisfy `input_range`. `Ns` is an integer pack `0, 1, ..., (sizeof...(Args) -
   1)`. `g` is an lvalue in the call expression if it is an lvalue in the call
   expression, and is an rvalue otherwise. Thus `std::move(g)(r)` can move the
   bound arguments into the call, where `g(r)` would copy. The specified
   template argument is (3) `C` or (4) the deduced type from a class template
   `C` that must not satisfy `view`. The program is ill-formed if `g` has
   volatile-qualified type.

### Exceptions

Only throws if construction of a non-view object throws.

### Notes

The insertion of elements into the container may involve copy which can be less
efficient than move because lvalue references are produced during the
indirection call. Users can opt-in to use `views::as_rvalue` to adapt the range
in order for their elements to always produce an rvalue reference during the
indirection call which implies move.

The parentheses are mandatory when using the pipe syntax.

```cpp
auto vec = r | std::ranges::to<std::vector>;   // error
auto vec = r | std::ranges::to<std::vector>(); // OK
```

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_to_container` | 202202L | (C++23) | `std::ranges::to`

### Example

A preview link: Compiler Explorer

```cpp
#include <concepts>
#include <list>
#include <print>
#include <ranges>
#include <vector>

int main()
{
    auto vec = std::views::iota(1, 5)
             | std::views::transform([](auto const v){ return v * 2; })
             | std::ranges::to<std::vector>();

    static_assert(std::same_as<decltype(vec), std::vector<int>>);
    std::println("{}", vec);

    auto lst = vec | std::views::take(3) | std::ranges::to<std::list<double>>();
    std::println("{}", lst);
}
```

Output:

```text
[2, 4, 6, 8]
[2, 4, 6]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/ranges/to*
