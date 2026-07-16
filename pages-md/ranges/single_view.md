# std::ranges::views::single, std::ranges::single_view

```cpp
template< std::copy_constructible T >
    requires std::is_object_v<T>
class single_view
    : public ranges::view_interface<single_view<T>>  // (1) (since C++20)
namespace views {
    inline constexpr /*unspecified*/ single = /*unspecified*/;
}  // (2) (since C++20)
Call signature
template< class T >
    requires /* see below */
constexpr /*see below*/ single( T&& t );  // (since C++20)
```

1) Produces a `view` that contains exactly one element of a specified value.

2) The expression `views::single(e)` is expression-equivalent to
   `single_view<std::decay_t<decltype((e))>>(e)` for any suitable subexpression
   `e`.

The lifetime of the element is bound to the parent `single_view`. Copying
`single_view` makes a copy of the element.

### Customization point objects

The name `views::single` denotes a *customization point object*, which is a
const function object of a literal `semiregular` class type. For exposition
purposes, the cv-unqualified version of its type is denoted as `__single_fn`.

All instances of `__single_fn` are equal. The effects of invoking different
instances of type `__single_fn` on the same arguments are equivalent, regardless
of whether the expression denoting the instance is an lvalue or rvalue, and is
const-qualified or not (however, a volatile-qualified instance is not required
to be invocable). Thus, `views::single` can be copied freely and its copies can
be used interchangeably.

Given a set of types `Args...`, if `std::declval<Args>()...` meet the
requirements for arguments to `views::single` above, `__single_fn` models

- `std::invocable<__single_fn, Args...>`,
- `std::invocable<const __single_fn, Args...>`,
- `std::invocable<__single_fn&, Args...>`, and
- `std::invocable<const __single_fn&, Args...>`.

Otherwise, no function call operator of `__single_fn` participates in overload
resolution.

### Data members

- **`value_` (private)** — An object of type `copyable-box``<T>`(until
  C++23)`movable-box``<T>`(since C++23). (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `single_view` (public member
  function)
- **begin (C++20)** — returns a pointer to the element (public member function)
- **end (C++20)** — returns a pointer past the element (public member function)
- **size [static] (C++20)** — returns 1 (one) (public static member function)
- **data (C++20)** — returns a pointer to the element (public member function)

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

## std::ranges::single_view::single_view

```cpp
single_view() requires std::default_initializable<T> = default;  // (1) (since C++20)
constexpr explicit single_view( const T& t );  // (2) (since C++20)
constexpr explicit single_view( T&& t );  // (3) (since C++20)
template< class... Args >
    requires std::constructible_from<T, Args...>
constexpr explicit single_view( std::in_place_t, Args&&... args );  // (4) (since C++20)
```

Constructs a `single_view`.

1) Default initializes `value_`, which value-initializes its contained value.

2) Initializes `value_` with `t`.

3) Initializes `value_` with `std::move(t)`.

4) Initializes `value_` as if by `value_{std::in_place,
   std::forward<Args>(args)...}`.

## std::ranges::single_view::begin

```cpp
constexpr T* begin() noexcept;
constexpr const T* begin() const noexcept;  // (since C++20)
```

Equivalent to `return data();`.

## std::ranges::single_view::end

```cpp
constexpr T* end() noexcept;
constexpr const T* end() const noexcept;  // (since C++20)
```

Equivalent to `return data() + 1;`.

## std::ranges::single_view::size

```cpp
static constexpr std::size_t size() noexcept;  // (since C++20)
```

Equivalent to `return 1;`.

This is a static function. This makes `single_view` model `/*tiny-range*/` as
required by `split_view`.

## std::ranges::single_view::data

```cpp
constexpr T* data() noexcept;
constexpr const T* data() const noexcept;  // (since C++20)
```

Returns a pointer to the contained value of `value_`. The behavior is undefined
if `value_` does not contains a value.

### Deduction guides

```cpp
template< class T >
single_view( T ) -> single_view<T>;  // (since C++20)
```

### Notes

For a `single_view`, the inherited `empty` member function always returns
`false`, and the inherited `operator bool` conversion function always returns
`true`.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <ranges>
#include <string>
#include <tuple>

int main()
{
    constexpr std::ranges::single_view sv1{3.1415}; // uses (const T&) constructor
    static_assert(sv1);
    static_assert(not sv1.empty());

    std::cout << "1) *sv1.data(): " << *sv1.data() << '\n'
              << "2) *sv1.begin(): " << *sv1.begin() << '\n'
              << "3)  sv1.size(): " << sv1.size() << '\n'
              << "4)  distance: " << std::distance(sv1.begin(), sv1.end()) << '\n';

    std::string str{"C++20"};
    std::cout << "5)  str = " << std::quoted(str) << '\n';
    std::ranges::single_view sv2{std::move(str)}; // uses (T&&) constructor
    std::cout << "6) *sv2.data(): " << std::quoted(*sv2.data()) << '\n'
              << "7)  str = " << std::quoted(str) << '\n';

    std::ranges::single_view<std::tuple<int, double, std::string>>
        sv3{std::in_place, 42, 3.14, "😄"}; // uses (std::in_place_t, Args&&... args)

    std::cout << "8)  sv3 holds a tuple: { "
              << std::get<0>(sv3[0]) << ", "
              << std::get<1>(sv3[0]) << ", "
              << std::get<2>(sv3[0]) << " }\n";
}
```

Output:

```text
1) *sv1.data(): 3.1415
2) *sv1.begin(): 3.1415
3)  sv1.size(): 1
4)  distance: 1
5)  str = "C++20"
6) *sv2.data(): "C++20"
7)  str = ""
8)  sv3 holds a tuple: { 42, 3.14, 😄 }
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3428 | C++20 | `single_view` was convertible from `std::in_place_t` | the
      constructor is made explicit
  P2367R0 | C++20 | deduction guides for `single_view` failed to decay the
      argument; `views::single` copied but not wrapped a `single_view` | a
      decaying guide provided; made always wrapping

### See also

- **ranges::empty_viewviews::empty (C++20)** — an empty `view` with no elements
  (class template) (variable template)
- **ranges::split_viewviews::split (C++20)** — a `view` over the subranges
  obtained from splitting another `view` using a delimiter (class template)
  (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/single_view*
