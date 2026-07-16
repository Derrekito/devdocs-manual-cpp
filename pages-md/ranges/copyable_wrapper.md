# Assignable wrapper (C++20)

```cpp
template<class T>
    requires std::copy_constructible<T> && std::is_object_v<T>
class /*copyable-box*/;  // (since C++20) (until C++23) (exposition only*)
template<class T>
    requires std::move_constructible<T> && std::is_object_v<T>
class /*movable-box*/;  // (since C++23) (exposition only*)
```

`ranges::single_view`, `ranges::repeat_view`,(since C++23) and range adaptors
that store an invocable object are specified in terms of an exposition-only
class template `copyable-box`(until C++23)`movable-box`(since C++23). The name
shown here is for exposition purposes only.

The wrapper behaves exactly like `std::optional<T>`, except that the default
constructor, copy assignment operator, and move assignment operator are
(conditionally) different from those of `std::optional`, which augments `T` with
assignability when needed and makes it always satisfy `copyable`or
`movable`(since C++23).

If `T` is already `copyable`, or both `std::is_nothrow_move_constructible_v<T>`
and `std::is_nothrow_copy_constructible_v<T>` are `true`, `/*copyable-box*/<T>`
may store only a `T` object, because it always contains a value.
*(until C++23)*

If `T`
- is already `copyable`, or
- is `copy_constructible` and both `std::is_nothrow_move_constructible_v<T>` and
  `std::is_nothrow_copy_constructible_v<T>` are `true`, or
- does not satisfy `copy_constructible` but satisfies `movable`, or
- does not satisfy `copy_constructible` but
  `std::is_nothrow_move_constructible_v<T>` is `true`,
`/*movable-box*/<T>` may store only a `T` object, because it always contains a
value.
*(since C++23)*

### Template parameters

- **T** — the type of the contained value, must be an object type that models
  `copy_constructible`(until C++23)`move_constructible`(since C++23)

### Member functions

## Default constructor

```cpp
constexpr /*copyable-box*/() noexcept(std::is_nothrow_default_constructible_v<T>)
    requires std::default_initializable<T>
    : /*copyable-box*/(std::in_place) { }  // (since C++20) (until C++23)
constexpr /*movable-box*/() noexcept(std::is_nothrow_default_constructible_v<T>)
    requires std::default_initializable<T>
    : /*movable-box*/(std::in_place) { }  // (since C++23)
```

The default constructor is provided if and only if `T` models
`default_initializable`.

A default-constructed wrapper contains a value-initialized `T` object.

## Assignment operators

```cpp
constexpr /*copyable-box*/& operator=(const /*copyable-box*/& other);
    noexcept(/* see below */);  // (since C++20) (until C++23)
constexpr /*movable-box*/& operator=(const /*movable-box*/& other);
    noexcept(/* see below */) requires std::copy_constructible<T>;  // (since C++23)
constexpr /*copyable-box*/& operator=(/*copyable-box*/&& other)
    noexcept(std::is_nothrow_move_constructible_v<T>);  // (since C++20) (until C++23)
constexpr /*movable-box*/& operator=(/*movable-box*/&& other)
    noexcept(std::is_nothrow_move_constructible_v<T>);  // (since C++23)
```

1) If `std::copyable<T>` is not modeled, the copy assignment operator is
   equivalently defined as: `constexpr /*copyable-box*/& operator=(const
   /*copyable-box*/& other) noexcept(std::is_nothrow_copy_constructible_v<T>) {
   if (this != std::addressof(other)) if (other) emplace(*other); else reset();
   return *this; }` (until C++23) `constexpr /*movable-box*/& operator=(const
   /*movable-box*/& other) noexcept(std::is_nothrow_copy_constructible_v<T>)
   requires std::copy_constructible<T> { if (this != std::addressof(other)) if
   (other) emplace(*other); else reset(); return *this; }` (since C++23)
   Otherwise, it is identical to the copy assignment operator of
   `std::optional`.

2) If `std::movable<T>` is not modeled, the move assignment operator is
   equivalently defined as: `constexpr /*copyable-box*/&
   operator=(/*copyable-box*/&& other)
   noexcept(std::is_nothrow_move_constructible_v<T>) { if (this !=
   std::addressof(other)) if (other) emplace(std::move(*other)); else reset();
   return *this; }` (until C++23) `constexpr /*movable-box*/&
   operator=(/*movable-box*/&& other)
   noexcept(std::is_nothrow_move_constructible_v<T>) { if (this !=
   std::addressof(other)) if (other) emplace(std::move(*other)); else reset();
   return *this; }` (since C++23) Otherwise, it is identical to the move
   assignment operator of `std::optional`.

## Members identical to std::optional

### Member functions

- **(constructor)** — constructs the optional object (public member function of
  `std::optional<T>`)
- **(destructor)** — destroys the contained value, if there is one (public
  member function of `std::optional<T>`)
- **operator=** — assigns contents (public member function of
  `std::optional<T>`)

**Observers**

- **operator->operator*** — accesses the contained value (public member function
  of `std::optional<T>`)
- **operator boolhas_value** — checks whether the object contains a value
  (public member function of `std::optional<T>`)

**Modifiers**

- **reset** — destroys any contained value (public member function of
  `std::optional<T>`)
- **emplace** — constructs the contained value in-place (public member function
  of `std::optional<T>`)

### Notes

A `copyable-box`(until C++23)`movable-box`(since C++23) does not contain a value
only if

- `T` does not model `movable` or `copyable`, and an exception is thrown on move
  assignment or copy assignment respectively, or
- it is initialized/assigned from another valueless wrapper.

Before P2325R3, the wrapper was called `semiregular-box` in the standard and
always satisfied `semiregular`, as the default constructor was always provided
(which might construct a valueless wrapper).

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges` | 201911L | (C++20) | Ranges library and constrained
      algorithms
  202106L | (C++20) (DR) | Non-default-initializable views
  202207L | (C++23) | Relaxing range adaptors to allow for move-only types

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2325R3 | C++20 | if `T` is not `default_initializable`, the default
      constructor constructs a wrapper which does not contain a value | the
      wrapper is also not `default_initializable`
  LWG 3572 | C++20 | conditionally different assignment operators were not
      constexpr | made constexpr

### See also

- **ranges::single_viewviews::single (C++20)** — a `view` that contains a single
  element of a specified value (class template) (customization point object)
- **ranges::repeat_viewviews::repeat (C++23)** — a `view` consisting of a
  generated sequence by repeatedly producing the same value (class template)
  (customization point object)
- **ranges::filter_viewviews::filter (C++20)** — a `view` that consists of the
  elements of a `range` that satisfies a predicate (class template) (range
  adaptor object)
- **ranges::transform_viewviews::transform (C++20)** — a `view` of a sequence
  that applies a transformation function to each element (class template) (range
  adaptor object)
- **ranges::take_while_viewviews::take_while (C++20)** — a `view` consisting of
  the initial elements of another `view`, until the first element on which a
  predicate returns `false` (class template) (range adaptor object)
- **ranges::drop_while_viewviews::drop_while (C++20)** — a `view` consisting of
  the elements of another `view`, skipping the initial subsequence of elements
  until the first element where the predicate returns `false` (class template)
  (range adaptor object)
- **ranges::zip_transform_viewviews::zip_transform (C++23)** — a `view`
  consisting of tuples of results of application of a transformation function to
  corresponding elements of the adapted views (class template) (customization
  point object)
- **ranges::adjacent_transform_viewviews::adjacent_transform (C++23)** — a
  `view` consisting of tuples of results of application of a transformation
  function to adjacent elements of the adapted view (class template) (range
  adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/copyable_wrapper*
