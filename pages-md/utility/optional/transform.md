# std::optional<T>::transform

```cpp
template< class F >
constexpr auto transform( F&& f ) &;  // (1) (since C++23)
template< class F >
constexpr auto transform( F&& f ) const&;  // (2) (since C++23)
template< class F >
constexpr auto transform( F&& f ) &&;  // (3) (since C++23)
template< class F >
constexpr auto transform( F&& f ) const&&;  // (4) (since C++23)
```

If `*this` contains a value, invokes `f` with the contained value as an
argument, and returns an `std::optional` that contains the result of that
invocation; otherwise, returns an empty `std::optional`.

The type of contained value in the result (denoted by `U` below) must be a
non-array object type, and must not be `std::in_place_t` or `std::nullopt_t`).
Otherwise, the program is ill-formed.

1) Let `U` be `std::remove_cv_t<std::invoke_result_t<F, T&>>`. If `*this`
   contains a value, returns a `std::optional<U>` whose contained value is
   direct-initialized from `std::invoke(std::forward<F>(f), **this)` (unlike
   `and_then()`, which must return an `std::optional` directly). Otherwise,
   returns an empty `std::optional<U>`. The program is ill-formed if the
   variable definition `U x(std::invoke(std::forward<F>(f), **this));` is
   ill-formed.

2) Same as (1), except that `U` is `std::remove_cv_t<std::invoke_result_t<F,
   const T&>>`.

3) Let `U` be `std::remove_cv_t<std::invoke_result_t<F, T>>`. If `*this`
   contains a value, returns a `std::optional<U>` whose contained value is
   direct-initialized from `std::invoke(std::forward<F>(f), std::move(**this))`.
   Otherwise, returns an empty `std::optional<U>`. The program is ill-formed if
   the variable definition `U x(std::invoke(std::forward<F>(f),
   std::move(**this)));` is ill-formed.

4) Same as (3), except that `U` is `std::remove_cv_t<std::invoke_result_t<F,
   const T>>`.

### Parameters

- **f** — a suitable function or Callable object whose call signature returns a
  non-reference type

### Return value

An `std::optional` containing the result of `f` or an empty `std::optional`, as
described above.

### Notes

Because `transform` directly constructs a `U` object at the right location,
rather than passing it to a constructor, `std::is_move_constructible_v<U>` can
be `false`.

As the callable `f` can't return a reference type, it cannot be a pointer to
data member.

Some languages call this operation *map*.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_optional` | 202110L | (C++23) | Monadic operations in
      `std::optional`

### Example

```cpp
#include <iostream>
#include <optional>

struct A { /* ... */ };
struct B { /* ... */ };
struct C { /* ... */ };
struct D { /* ... */ };

auto A_to_B(A) -> B { /* ... */ std::cout << "A => B \n"; return {}; }
auto B_to_C(B) -> C { /* ... */ std::cout << "B => C \n"; return {}; }
auto C_to_D(C) -> D { /* ... */ std::cout << "C => D \n"; return {}; }

void try_transform_A_to_D(std::optional<A> o_A)
{
    std::cout << (o_A ? "o_A has a value\n" : "o_A is empty\n");

    std::optional<D> o_D = o_A.transform(A_to_B)
                              .transform(B_to_C)
                              .transform(C_to_D);

    std::cout << (o_D ? "o_D has a value\n\n" : "o_D is empty\n\n");
};

int main()
{
    try_transform_A_to_D( A{} );
    try_transform_A_to_D( {} );
}
```

Output:

```text
o_A has a value
A => B
B => C
C => D
o_D has a value

o_A is empty
o_D is empty
```

### See also

- **value_or** — returns the contained value if available, another value
  otherwise (public member function)
- **and_then (C++23)** — returns the result of the given function on the
  contained value if it exists, or an empty `optional` otherwise (public member
  function)
- **or_else (C++23)** — returns the `optional` itself if it contains a value, or
  the result of the given function otherwise (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/optional/transform*
