# std::optional<T>::operator=

```cpp
optional& operator=( std::nullopt_t ) noexcept;  // (since C++17) (until C++20)
constexpr optional& operator=( std::nullopt_t ) noexcept;  // (since C++20)
constexpr optional& operator=( const optional& other );  // (2) (since C++17)
constexpr optional& operator=( optional&& other ) noexcept(/* see below */);  // (3) (since C++17)
template< class U = T >
optional& operator=( U&& value );  // (since C++17) (until C++20)
template< class U = T >
constexpr optional& operator=( U&& value );  // (since C++20)
template< class U >
optional& operator=( const optional<U>& other );  // (since C++17) (until C++20)
template< class U >
constexpr optional& operator=( const optional<U>& other );  // (since C++20)
template< class U >
optional& operator=( optional<U>&& other );  // (since C++17) (until C++20)
template< class U >
constexpr optional& operator=( optional<U>&& other );  // (since C++20)
```

Replaces contents of `*this` with the contents of `other`.

1) If `*this` contains a value before the call, the contained value is destroyed
   by calling its destructor as if by `value().T::~T()`. `*this` does not
   contain a value after this call.
2,3) Assigns the state of `other`.

- If both `*this` and `other` do not contain a value, the function has no
  effect.
- If `*this` contains a value, but `other` does not, then the contained value is
  destroyed by calling its destructor. `*this` does not contain a value after
  the call.
- If `other` contains a value, then depending on whether `*this` contains a
  value, the contained value is either direct-initialized or assigned from
  `*other` (2) or `std::move(*other)` (3). Note that a moved-from optional still
  *contains a value*.
- Overload (2) is deleted when either `std::is_copy_constructible_v<T>` or
  `std::is_copy_assignable_v<T>` is `false`. It is trivial if
  `std::is_trivially_copy_constructible_v<T>`,
  `std::is_trivially_copy_assignable_v<T>` and
  `std::is_trivially_destructible_v<T>` are all `true`.
- Overload (3) does not participate in overload resolution when either
  `std::is_move_constructible_v<T>` or `std::is_move_assignable_v<T>` is
  `false`. It is trivial if `std::is_trivially_move_constructible_v<T>`,
  `std::is_trivially_move_assignable_v<T>` and
  `std::is_trivially_destructible_v<T>` are all `true`.

4) Perfect-forwarded assignment: depending on whether `*this` contains a value
before the call, the contained value is either direct-initialized from
`std::forward<U>(value)` or assigned from `std::forward<U>(value)`. The function
does not participate in overload resolution unless `std::decay_t<U>`(until
C++20)`std::remove_cvref_t<U>`(since C++20) is not `std::optional<T>`,
`std::is_constructible_v<T, U>` is `true`, `std::is_assignable_v<T&, U>` is
`true`, and at least one of the following is true:

- `T` is not a scalar type;
- `std::decay_t<U>` is not `T`.

5,6) Assigns the state of `other`.

- If both `*this` and `other` do not contain a value, the function has no
  effect.
- If `*this` contains a value, but `other` does not, then the contained value is
  destroyed by calling its destructor. `*this` does not contain a value after
  the call.
- If `other` contains a value, then depending on whether `*this` contains a
  value, the contained value is either direct-initialized or assigned from
  `*other` (5) or `std::move(*other)` (6). Note that a moved-from optional still
  *contains a value*.
- These overloads do not participate in overload resolution unless the following
  conditions are met: `T` is not constructible, convertible, or assignable from
  any expression of type (possibly `const`) `std::optional<U>`, i.e., the
  following 12 type traits are all `false`: `std::is_constructible_v<T,
  std::optional<U>&>` `std::is_constructible_v<T, const std::optional<U>&>`
  `std::is_constructible_v<T, std::optional<U>&&>` `std::is_constructible_v<T,
  const std::optional<U>&&>` `std::is_convertible_v<std::optional<U>&, T>`
  `std::is_convertible_v<const std::optional<U>&, T>`
  `std::is_convertible_v<std::optional<U>&&, T>` `std::is_convertible_v<const
  std::optional<U>&&, T>` `std::is_assignable_v<T&, std::optional<U>&>`
  `std::is_assignable_v<T&, const std::optional<U>&>` `std::is_assignable_v<T&,
  std::optional<U>&&>` `std::is_assignable_v<T&, const std::optional<U>&&>` For
  overload (5), `std::is_constructible_v<T, const U&>` and
  `std::is_assignable_v<T&, const U&>` are both `true`. For overload (6),
  `std::is_constructible_v<T, U>` and `std::is_assignable_v<T&, U>` are both
  `true`.

### Parameters

- **other** — another `optional` object whose contained value to assign
- **value** — value to assign to the contained value

### Return value

`*this`

### Exceptions

2-6) Throws any exception thrown by the constructor or assignment operator of
   `T`. If an exception is thrown, the initialization state of `*this` (and of
   `other` in case of (2,3) and (5,6)) is unchanged, i.e. if the object
   contained a value, it still contains a value, and the other way round. The
   contents of `value` and the contained values of `*this` and `other` depend on
   the exception safety guarantees of the operation from which the exception
   originates (copy-constructor, move-assignment, etc.).

3) Has following `noexcept` specification:
   `noexcept(std::is_nothrow_move_assignable_v<T> &&
   std::is_nothrow_move_constructible_v<T>)`

### Notes

An optional object `op` may be turned into an empty optional with both `op =
{};` and `op = nullopt;`. The first expression constructs an empty `optional`
object with `{}` and assigns it to `op`.

### Example

```cpp
#include <iostream>
#include <optional>

int main()
{
    std::optional<const char*> s1 = "abc", s2; // constructor
    s2 = s1; // assignment
    s1 = "def"; // decaying assignment (U = char[4], T = const char*)
    std::cout << *s2 << ' ' << *s1 << '\n';
}
```

Output:

```text
abc def
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0602R4 | C++17 | copy/move assignment operator may not be trivial even if
      underlying operations are trivial | required to propagate triviality
  P2231R1 | C++20 | converting assignment operators were not constexpr while the
      required operations can be in C++20 | made constexpr

### See also

- **emplace** — constructs the contained value in-place (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/operator%3D*
