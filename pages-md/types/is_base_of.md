# std::is_base_of

```cpp
template< class Base, class Derived >
struct is_base_of;  // (since C++11)
```

`std::is_base_of` is a BinaryTypeTrait.

If `Derived` is derived from `Base` or if both are the same non-union class (in
both cases ignoring cv-qualification), provides the member constant `value`
equal to `true`. Otherwise `value` is `false`.

If both `Base` and `Derived` are non-union class types, and they are not the
same type (ignoring cv-qualification), `Derived` should be a complete type;
otherwise the behavior is undefined.

The behavior of a program that adds specializations for `std::is_base_of` or
`std::is_base_of_v`(since C++17) is undefined.

### Helper variable template

```cpp
template< class Base, class Derived >
inline constexpr bool is_base_of_v = is_base_of<Base, Derived>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `Derived` is derived from `Base` or if both are
  the same non-union class (in both cases ignoring cv-qualification), `false`
  otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

`std::is_base_of<A, B>::value` is `true` even if `A` is a private, protected, or
ambiguous base class of `B`. In many situations, `std::is_convertible<B*, A*>`
is the more appropriate test.

Although no class is its own base, `std::is_base_of<T, T>::value` is true
because the intent of the trait is to model the "is-a" relationship, and `T` is
a `T`. Despite that, `std::is_base_of<int, int>::value` is `false` because only
classes participate in the relationship that this trait models.

### Possible Implementation

```cpp
namespace details
{
    template<typename B>
    std::true_type test_ptr_conv(const volatile B*);
    template<typename>
    std::false_type test_ptr_conv(const volatile void*);

    template<typename B, typename D>
    auto test_is_base_of(int) -> decltype(test_ptr_conv<B>(static_cast<D*>(nullptr)));
    template<typename, typename>
    auto test_is_base_of(...) -> std::true_type; // private or ambiguous base
}

template<typename Base, typename Derived>
struct is_base_of :
    std::integral_constant<
        bool,
        std::is_class<Base>::value &&
        std::is_class<Derived>::value &&
        decltype(details::test_is_base_of<Base, Derived>(0))::value
    > {};
```

### Example

```cpp
#include <type_traits>

class A {};
class B : A {};
class C : B {};
class D {};
union E {};
using I = int;

static_assert
(
    std::is_base_of_v<A, A> == true &&
    std::is_base_of_v<A, B> == true &&
    std::is_base_of_v<A, C> == true &&
    std::is_base_of_v<A, D> != true &&
    std::is_base_of_v<B, A> != true &&
    std::is_base_of_v<E, E> != true &&
    std::is_base_of_v<I, I> != true
);

int main() {}
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | the behavior might be undefined if `Derived` is an
      incomplete union type | the base characteristic is `std::false_type` in
      this case

### See also

- **is_convertibleis_nothrow_convertible (C++11)(C++20)** — checks if a type can
  be converted to the other type (class template)
- **derived_from (C++20)** — specifies that a type is derived from another type
  (concept)

---
*Source: https://en.cppreference.com/w/cpp/types/is_base_of*
