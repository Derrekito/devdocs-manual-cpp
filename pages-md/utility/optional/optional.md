# std::optional<T>::optional

```cpp
constexpr optional() noexcept;
constexpr optional( std::nullopt_t ) noexcept;  // (1) (since C++17)
constexpr optional( const optional& other );  // (2) (since C++17)
constexpr optional( optional&& other ) noexcept(/* see below */);  // (3) (since C++17)
template < class U >
optional( const optional<U>& other );  // (since C++17) (until C++20) (conditionally explicit)
template < class U >
constexpr optional( const optional<U>& other );  // (since C++20) (conditionally explicit)
template < class U >
optional( optional<U>&& other );  // (since C++17) (until C++20) (conditionally explicit)
template < class U >
constexpr optional( optional<U>&& other );  // (since C++20) (conditionally explicit)
template< class... Args >
constexpr explicit optional( std::in_place_t, Args&&... args );  // (6) (since C++17)
template< class U, class... Args >
constexpr explicit optional( std::in_place_t,
                             std::initializer_list<U> ilist,
                             Args&&... args );  // (7) (since C++17)
template < class U = T >
constexpr optional( U&& value );  // (8) (since C++17) (conditionally explicit)
```

Constructs a new optional object.

1) Constructs an object that *does not contain* a value.
2) Copy constructor: If `other` contains a value, initializes the contained
value as if direct-initializing (but not direct-list-initializing) an object of
type `T` with the expression `*other`. If `other` does not contain a value,
constructs an object that *does not contain* a value.

- This constructor is defined as deleted if `std::is_copy_constructible_v<T>` is
  `false`.
- It is a trivial constructor if `std::is_trivially_copy_constructible_v<T>` is
  `true`.

3) Move constructor: If `other` contains a value, initializes the contained
value as if direct-initializing (but not direct-list-initializing) an object of
type `T` with the expression `std::move(*other)` and *does not* make `other`
empty: a moved-from `std::optional` still *contains* a value, but the value
itself is moved from. If `other` does not contain a value, constructs an object
that *does not contain* a value.

- This constructor does not participate in overload resolution unless
  `std::is_move_constructible_v<T>` is `true`.
- It is a trivial constructor if `std::is_trivially_move_constructible_v<T>` is
  `true`.

4) Converting copy constructor: If `other` does not contain a value, constructs
an optional object that *does not contain* a value. Otherwise, constructs an
optional object that *contains* a value, initialized as if direct-initializing
(but not direct-list-initializing) an object of type `T` with the expression
`*other`.

- This constructor does not participate in overload resolution unless the
  following conditions are met: `std::is_constructible_v<T, const U&>` is
  `true`. If `T` is not (possibly cv-qualified) bool, `T` is not constructible
  or convertible from any expression of type (possibly const) std::optional<U>,
  i.e., the following 8 values are all `false`: `std::is_constructible_v<T,
  std::optional<U>&>` `std::is_constructible_v<T, const std::optional<U>&>`
  `std::is_constructible_v<T, std::optional<U>&&>` `std::is_constructible_v<T,
  const std::optional<U>&&>` `std::is_convertible_v<std::optional<U>&, T>`
  `std::is_convertible_v<const std::optional<U>&, T>`
  `std::is_convertible_v<std::optional<U>&&, T>` `std::is_convertible_v<const
  std::optional<U>&&, T>`
- This constructor is explicit if and only if `std::is_convertible_v<const U&,
  T>` is `false`.

5) Converting move constructor: If `other` does not contain a value, constructs
an optional object that *does not contain* a value. Otherwise, constructs an
optional object that *contains* a value, initialized as if direct-initializing
(but not direct-list-initializing) an object of type `T` with the expression
`std::move(*other)`.

- This constructor does not participate in overload resolution unless the
  following conditions are met: `std::is_constructible_v<T, U&&>` is `true`. If
  `T` is not (possibly cv-qualified) bool, `T` is not constructible or
  convertible from any expression of type (possibly const) std::optional<U>,
  i.e., the following 8 values are all `false`: `std::is_constructible_v<T,
  std::optional<U>&>` `std::is_constructible_v<T, const std::optional<U>&>`
  `std::is_constructible_v<T, std::optional<U>&&>` `std::is_constructible_v<T,
  const std::optional<U>&&>` `std::is_convertible_v<std::optional<U>&, T>`
  `std::is_convertible_v<const std::optional<U>&, T>`
  `std::is_convertible_v<std::optional<U>&&, T>` `std::is_convertible_v<const
  std::optional<U>&&, T>`
- This constructor is explicit if and only if `std::is_convertible_v<U&&, T>` is
  `false`.

6) Constructs an optional object that *contains* a value, initialized as if
direct-initializing (but not direct-list-initializing) an object of type `T`
from the arguments `std::forward<Args>(args)...`.

- If the selected constructor of `T` is a constexpr constructor, this
  constructor is a constexpr constructor.
- The function does not participate in the overload resolution unless
  `std::is_constructible_v<T, Args...>` is `true`.

7) Constructs an optional object that *contains* a value, initialized as if
direct-initializing (but not direct-list-initializing) an object of type `T`
from the arguments `ilist, std::forward<Args>(args)...`.

- If the selected constructor of `T` is a constexpr constructor, this
  constructor is a constexpr constructor.
- The function does not participate in the overload resolution unless
  `std::is_constructible_v<T, std::initializer_list<U>&, Args...>` is `true`.

8) Constructs an optional object that *contains* a value, initialized as if
direct-initializing (but not direct-list-initializing) an object of type `T`
with the expression `std::forward<U>(value)`.

- If the selected constructor of `T` is a constexpr constructor, this
  constructor is a constexpr constructor.
- This constructor does not participate in overload resolution unless the
  following conditions are met: `std::is_constructible_v<T, U&&>` is `true`.
  `std::decay_t<U>`(until C++20)`std::remove_cvref_t<U>`(since C++20) is neither
  `std::in_place_t` nor std::optional<T>. If `T` is (possibly cv-qualified)
  bool, `std::decay_t<U>`(until C++20)`std::remove_cvref_t<U>`(since C++20) is
  not a specialization of `std::optional`.
- This constructor is explicit if and only if `std::is_convertible_v<U&&, T>` is
  false.

### Parameters

- **other** — another optional object whose contained value is copied
- **value** — value with which to initialize the contained value
- **args...** — arguments with which to initialize the contained value
- **ilist** — initializer list with which to initialize the contained value

### Exceptions

2) Throws any exception thrown by the constructor of `T`.

3) Throws any exception thrown by the constructor of `T`. Has the following
   `noexcept` specification:
   `noexcept(std::is_nothrow_move_constructible<T>::value)`

4-8) Throws any exception thrown by the constructor of `T`.

### Deduction guides

### Notes

Before the resolution of LWG issue 3836, constructing an std::optional<bool>
from std::optional<U> would select overload (8) instead of overloads (4,5) if
`U` is not bool. This is because overloads (4,5) did not participate in overload
resolution if `T` (bool in this case) can be constructed or converted from
std::optional<U>, but `std::optional::operator bool` makes the conversion
possible for any `U`.

As a result, the constructed std::optional<bool> always contains a value. That
value is determined by whether the provided std::optional<U> object contains a
value, rather than the bool value direct-initialized from the contained value:

```cpp
std::optional<bool> op_false(false);
std::optional<int> op_zero(0);

std::optional<int> from_bool(op_false); // OK: contains 0 (initialized from false)
std::optional<bool> from_int(op_0);     // DEFECT (LWG 3836): contains true
                                        // because op_0 contains a value, even if
                                        // initializing bool from that value gives false
```

### Example

```cpp
#include <iostream>
#include <optional>
#include <string>

int main()
{
    std::optional<int> o1, // empty
                       o2 = 1, // init from rvalue
                       o3 = o2; // copy-constructor

    // calls std::string( initializer_list<CharT> ) constructor
    std::optional<std::string> o4(std::in_place, {'a', 'b', 'c'});

    // calls std::string( size_type count, CharT ch ) constructor
    std::optional<std::string> o5(std::in_place, 3, 'A');

    // Move-constructed from std::string using deduction guide to pick the type

    std::optional o6(std::string{"deduction"});

    std::cout << *o2 << ' ' << *o3 << ' ' << *o4 << ' ' << *o5  << ' ' << *o6 << '\n';
}
```

Output:

```text
1 1 abc AAA deduction
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3836 | C++17 | when constructing an std::optional<bool> from
      std::optional<U>, the overload resolution would select overload (8) if `U`
      is not bool | always selects the converting copy/move constructor in this
      case
  P0602R4 | C++17 | copy/move constructors might not be trivial even if
      underlying constructor is trivial | required to propagate triviality
  P2231R1 | C++20 | converting constructors from another `std::optional` was not
      constexpr while the required operations can be in C++20 | made constexpr

### See also

- **make_optional (C++17)** — creates an `optional` object (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/optional*
