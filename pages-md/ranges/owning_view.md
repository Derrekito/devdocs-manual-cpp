# std::ranges::owning_view

```cpp
template< ranges::range R >
    requires std::movable<R> && (!/*is-initializer-list*/<R>)
class owning_view
    : public ranges::view_interface<owning_view<R>>  // (since C++20)
```

`owning_view` is a `view` that has unique ownership of a `range`. It is
move-only and stores that `range` within it.

The constant `/*is-initializer-list*/<R>` in the requires-clause is `true` if
and only if `std::remove_cvref_t<R>` is a specialization of
`std::initializer_list`.

### Data members

- **`r_` (private)** — The underlying range of type `R`. (exposition-only member
  object*)

### Member functions

- **(constructor) (C++20)** — constructs an `owning_view` by value-initializing
  or move-constructing the stored range (public member function)
- **operator= (C++20)** — move-assigns the stored range (public member function)
- **base (C++20)** — returns a reference to the stored range (public member
  function)
- **begin (C++20)** — returns the beginning iterator of the stored range (public
  member function)
- **end (C++20)** — returns the sentinel of the stored range (public member
  function)
- **empty (C++20)** — checks whether the stored range is empty (public member
  function)
- **size (C++20)** — returns the size of the stored `sized_range` (public member
  function)
- **data (C++20)** — returns the pointer to the beginning of the stored
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

## std::ranges::owning_view::owning_view

```cpp
owning_view() requires std::default_initializable<R> = default;  // (1) (since C++20)
owning_view( owning_view&& other ) = default;  // (2) (since C++20)
constexpr owning_view( R&& t );  // (3) (since C++20)
owning_view( const owning_view& ) = delete;  // (4) (since C++20)
```

1) Default constructor. Value-initializes the stored range by its default member
   initializer (`= R()`).

2) Move constructor. Move constructs the stored range from that of `other`.

3) Move constructs the stored range from `t`.

4) Copy constructor is deleted. `owning_view` is move-only.

### Parameters

- **other** — another `owning_view` to move from
- **t** — range to move from

## std::ranges::owning_view::operator=

```cpp
owning_view& operator=( owning_view&& other ) = default;  // (1) (since C++20)
owning_view& operator=( const owning_view& ) = delete;  // (2) (since C++20)
```

1) Move assignment operator. Move assigns the stored range from that of `other`.

2) Copy assignment operator is deleted. `owning_view` is move-only.

### Parameters

- **other** — another `owning_view` to move from

### Return value

`*this`

## std::ranges::owning_view::base

```cpp
constexpr R& base() & noexcept;  // (1) (since C++20)
constexpr const R& base() const & noexcept;  // (2) (since C++20)
constexpr R&& base() && noexcept;  // (3) (since C++20)
constexpr const R&& base() const && noexcept;  // (4) (since C++20)
```

Returns a reference to the stored range, keeping value category and
const-qualification.

1,2) Equivalent to `return r_;`.

3,4) Equivalent to `return std::move(r_);`.

## std::ranges::owning_view::begin

```cpp
constexpr ranges::iterator_t<R> begin();  // (1) (since C++20)
constexpr auto begin() const requires ranges::range<const R>;  // (2) (since C++20)
```

Equivalent to `return ranges::begin(r_);`.

## std::ranges::owning_view::end

```cpp
constexpr ranges::sentinel_t<R> end();  // (1) (since C++20)
constexpr auto end() const requires ranges::range<const R>;  // (2) (since C++20)
```

Equivalent to `return ranges::end(r_);`.

## std::ranges::owning_view::empty

```cpp
constexpr bool empty() requires requires { ranges::empty(r_); };  // (1) (since C++20)
constexpr bool empty() const requires requires { ranges::empty(r_); };  // (2) (since C++20)
```

Equivalent to `return ranges::empty(r_);`.

## std::ranges::owning_view::size

```cpp
constexpr auto size() requires ranges::sized_range<R>;  // (1) (since C++20)
constexpr auto size() const requires ranges::sized_range<const R>;  // (2) (since C++20)
```

Equivalent to `return ranges::size(r_);`.

## std::ranges::owning_view::data

```cpp
constexpr auto data() requires ranges::contiguous_range<R>;  // (1) (since C++20)
constexpr auto data() const requires ranges::contiguous_range<const R>;  // (2) (since C++20)
```

Equivalent to `return ranges::data(r_);`.

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<std::ranges::owning_view<T>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `owning_view`
satisfy `borrowed_range` when the underlying range satisfies it.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <ranges>
#include <string>

int main()
{
    using namespace std::literals;
    std::ranges::owning_view ov{"cosmos"s}; // the deduced type of R is std::string;
                                            // `ov` is the only owner of this string
    assert(
        ov.empty() == false &&
        ov.size() == 6 &&
        ov.size() == ov.base().size() &&
        ov.front() == 'c' &&
        ov.front() == *ov.begin() &&
        ov.back() == 's' &&
        ov.back() == *(ov.end() - 1) &&
        ov.data() == ov.base()
    );

    std::cout << "sizeof(ov): " << sizeof ov << '\n' // typically equal to sizeof(R)
              << "range-for: ";
    for (const char ch : ov)
        std::cout << ch;
    std::cout << '\n';

    std::ranges::owning_view<std::string> ov2;
    assert(ov2.empty());
//  ov2 = ov; // compile-time error: copy assignment operator is deleted
    ov2 = std::move(ov); // OK
    assert(ov2.size() == 6);
}
```

Possible output:

```text
sizeof(ov): 32
range-for: cosmos
```

### See also

- **ranges::ref_view (C++20)** — a `view` of the elements of some other `range`
  (class template)
- **views::all_tviews::all (C++20)** — a `view` that includes all elements of a
  `range` (alias template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/owning_view*
