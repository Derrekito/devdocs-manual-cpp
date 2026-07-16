# std::ranges::ref_view

```cpp
template< ranges::range R >
    requires std::is_object_v<R>
class ref_view
    : public ranges::view_interface<ref_view<R>>  // (since C++20)
```

`ref_view` is a `view` of the elements of some other `range`. It wraps a
reference to that `range`.

### Data members

- **`r_` (private)** — A pointer of type `R*` to the underlying range.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `ref_view` that references to the
  given range (public member function)
- **base (C++20)** — returns the references to the referenced range (public
  member function)
- **begin (C++20)** — returns the beginning iterator of the referenced range
  (public member function)
- **end (C++20)** — returns the sentinel of the referenced range (public member
  function)
- **empty (C++20)** — checks whether the referenced range is empty (public
  member function)
- **size (C++20)** — returns the size of the referenced `sized_range` (public
  member function)
- **data (C++20)** — returns the pointer to the beginning of the referenced
  `contiguous_range` (public member function)

**Inherited from `std::ranges::view_interface`**

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

## std::ranges::ref_view::ref_view

```cpp
template< /*different-from*/<ref_view> T >
    requires std::convertible_to<T, R&> && requires { _FUN(std::declval<T>()); }
constexpr ref_view( T&& t );  // (since C++20)
```

Initializes `r_` with `std::addressof(static_cast<R&>(std::forward<T>(t)))`.

`/*different-from*/<T, U>` is satisfied if and only if `std::remove_cvref_t<T>`
and `std::remove_cvref_t<U>` are not the same type, and overloads of `_FUN` are
declared as `void _FUN(R&); void _FUN(R&&) = delete;`.

### Parameters

- **t** — range to reference

## std::ranges::ref_view::base

```cpp
constexpr R& base() const;  // (since C++20)
```

Equivalent to `return *r_;`.

## std::ranges::ref_view::begin

```cpp
constexpr ranges::iterator_t<R> begin() const;  // (since C++20)
```

Equivalent to `return ranges::begin(*r_);`.

## std::ranges::ref_view::end

```cpp
constexpr ranges::sentinel_t<R> end() const;  // (since C++20)
```

Equivalent to `return ranges::end(*r_);`.

## std::ranges::ref_view::empty

```cpp
constexpr bool empty() const
    requires requires { ranges::empty(*r_); };  // (since C++20)
```

Equivalent to `return ranges::empty(*r_);`.

## std::ranges::ref_view::size

```cpp
constexpr auto size() const
    requires ranges::sized_range<R>;  // (since C++20)
```

Equivalent to `return ranges::size(*r_);`.

## std::ranges::ref_view::data

```cpp
constexpr auto data() const
    requires ranges::contiguous_range<R>;  // (since C++20)
```

Equivalent to `return ranges::data(*r_);`.

### Deduction guides

```cpp
template< class R >
ref_view( R& ) -> ref_view<R>;  // (since C++20)
```

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<ranges::ref_view<T>> = true;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `ref_view`
satisfy `borrowed_range`.

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    const std::string s{"cosmos"};

    const std::ranges::take_view tv{s, 3};
    const std::ranges::ref_view rv{tv};

    std::cout
        << std::boolalpha
        << "call empty() : " << rv.empty() << '\n'
        << "call size()  : " << rv.size() << '\n'
        << "call begin() : " << *rv.begin() << '\n'
        << "call end()   : " << *(rv.end() - 1) << '\n'
        << "call data()  : " << rv.data() << '\n'
        << "call base()  : " << rv.base().size() << '\n' // ~> tv.size()
        << "range-for    : ";

    for (const auto c : rv)
        std::cout << c;
    std::cout << '\n';
}
```

Output:

```text
call empty() : false
call size()  : 3
call begin() : c
call end()   : s
call data()  : cosmos
call base()  : 3
range-for    : cos
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2325R3 | C++20 | default constructor was provided as `view` must be
      `default_initializable` | removed along with the requirement

### See also

- **reference_wrapper (C++11)** — CopyConstructible and CopyAssignable reference
  wrapper (class template)
- **ranges::owning_view (C++20)** — a `view` with unique ownership of some
  `range` (class template)
- **views::all_tviews::all (C++20)** — a `view` that includes all elements of a
  `range` (alias template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/ref_view*
