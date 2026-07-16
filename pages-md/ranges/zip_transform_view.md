# std::ranges::views::zip_transform, std::ranges::zip_transform_view

```cpp
template< std::copy_constructible F, ranges::input_range... Views >
    requires (ranges::view<Views> && ...) && (sizeof...(Views) > 0) &&
              std::is_object_v<F> && std::regular_invocable<
                  F&, ranges::range_reference_t<Views>...> &&
              /*can-reference*/<std::invoke_result_t<
                  F&, ranges::range_reference_t<Views>...>>
class zip_transform_view
     : public ranges::view_interface<zip_transform_view<F, Views...>>  // (1) (since C++23)
namespace views {
    inline constexpr /*unspecified*/ zip_transform = /*unspecified*/;
}  // (2) (since C++23)
Call signature
template< class F, ranges::viewable_range... Rs >
    requires /* see below */
constexpr auto zip_transform( F&& f, Rs&&... rs );  // (since C++23)
```

1) `zip_transform_view` is a range adaptor that takes an invocable object and
   one or more `view`s, and produces a `view` whose `i`*th* element is the
   result of applying the invocable object to the `i`*th* elements of all views.
   A type `T` models the exposition-only concept `/*can-reference*/` if and only
   if `T&` is a valid type.
2) `views::zip_transform` is a customization point object.

When calling with one argument `f`, let `FD` be `std::decay_t<decltype(f)>`, if:

- `FD` models `copy_constructible`,
- `FD&` models `regular_invocable`, and
- `std::invoke_result_t<FD&>` is an object type,

then `views::zip_transform(f)` is expression-equivalent to `((void)f,
auto(views::empty<std::decay_t<std::invoke_result_t<FD&>>>))`. Otherwise, the
call to `views::zip_transform` is ill-formed. When calling with more than one
arguments `f` and `rs...`, `views::zip_transform(f, rs...)` is
expression-equivalent to `ranges::zip_transform_view(f, rs...)`.

`zip_transform_view` models the concepts `random_access_range`,
`bidirectional_range`, `forward_range`, `input_range`, `common_range`, and
`sized_range` when the underlying `ranges::zip_view<Views...>` models respective
concepts.

### Customization point objects

The name `views::zip_transform` denotes a *customization point object*, which is
a const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as
`__zip_transform_fn`.

All instances of `__zip_transform_fn` are equal. The effects of invoking
different instances of type `__zip_transform_fn` on the same arguments are
equivalent, regardless of whether the expression denoting the instance is an
lvalue or rvalue, and is const-qualified or not (however, a volatile-qualified
instance is not required to be invocable). Thus, `views::zip_transform` can be
copied freely and its copies can be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::zip_transform` above, `__zip_transform_fn`
models

- `std::invocable<__zip_transform_fn, Args...>`,
- `std::invocable<const __zip_transform_fn, Args...>`,
- `std::invocable<__zip_transform_fn&, Args...>`, and
- `std::invocable<const __zip_transform_fn&, Args...>`.

Otherwise, no function call operator of `__zip_transform_fn` participates in
overload resolution.

### Member functions

- **(constructor) (C++23)** — constructs a `zip_transform_view` (public member
  function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++23)** — returns the number of elements. Provided only if each
  underlying (adapted) range satisfies `sized_range`. (public member function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Member types

- **`InnerView` (private)** — `ranges::zip_view<Views...>`. (exposition-only
  member type*)
- **`ziperator` (private)** — `ranges::iterator_t<const InnerView>` if `Const`
  is `true`, otherwise `ranges::iterator_t<InnerView>`. (exposition-only member
  type*)
- **`zentinel` (private)** — `ranges::sentinel_t<const InnerView>` if `Const` is
  `true`, otherwise `ranges::sentinel_t<InnerView>`. (exposition-only member
  type*)

### Data members

- **`zip_` (private)** — An underlying view object of type `InnerView`
  (exposition-only member object*)
- **`fun_` (private)** — A wrapped invocable object of type `movable-box<F>`
  (exposition-only member object*)

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++23)** — the sentinel type used when the underlying `zip_view`
  is not a `common_range` (exposition-only member class template*)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_zip` | 202110L | (C++23) | `std::ranges::zip_view`,
      `std::ranges::zip_transform_view`, `std::ranges::adjacent_view`,
      `std::ranges::adjacent_transform_view`

### Example

```cpp
#include <array>
#include <iostream>
#include <list>
#include <ranges>
#include <vector>

void print(auto const rem, auto const& r)
{
    std::cout << rem << '{';
    for (char o[]{0,' ',0}; auto const& e : r)
        std::cout << o << e, *o = ',';
    std::cout << "}\n";
}

int main()
{
    auto v1 = std::vector<float>{1, 2, 3};
    auto v2 = std::list<short>{1, 2, 3, 4};
    auto v3 = std::to_array({1, 2, 3, 4, 5});

    auto add = [](auto a, auto b, auto c) { return a + b + c; };

    auto sum = std::views::zip_transform(add, v1, v2, v3);

    print("v1:  ", v1);
    print("v2:  ", v2);
    print("v3:  ", v3);
    print("sum: ", sum);
}
```

Output:

```text
v1:  {1, 2, 3}
v2:  {1, 2, 3, 4}
v3:  {1, 2, 3, 4, 5}
sum: {3, 6, 9}
```

### See also

- **ranges::zip_viewviews::zip (C++23)** — a `view` consisting of tuples of
  references to corresponding elements of the adapted views (class template)
  (customization point object)
- **ranges::transform_viewviews::transform (C++20)** — a `view` of a sequence
  that applies a transformation function to each element (class template) (range
  adaptor object)
- **ranges::elements_viewviews::elements (C++20)** — takes a `view` consisting
  of `tuple-like` values and a number N and produces a `view` of Nth element of
  each tuple (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view*
