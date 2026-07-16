# std::is_empty

```cpp
template< class T >
struct is_empty;  // (since C++11)
```

`std::is_empty` is a UnaryTypeTrait.

If `T` is an empty type (that is, a non-union class type with no non-static data
members other than bit-fields of size 0, no virtual functions, no virtual base
classes, and no non-empty base classes), provides the member constant `value`
equal to `true`. For any other type, `value` is `false`.

If `T` is an incomplete non-union class type, the behavior is undefined.

The behavior of a program that adds specializations for `std::is_empty` or
`std::is_empty_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper variable template

```cpp
template< class T >
inline constexpr bool is_empty_v = is_empty<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is an empty class type, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Notes

Inheriting from empty base classes usually does not increase the size of a class
due to empty base optimization.

`std::is_empty<T>` and all other type traits are empty classes.

### Example

```cpp
#include <iostream>
#include <type_traits>

struct A {};
static_assert(std::is_empty_v<A> == true);

struct B { int m; };
static_assert(std::is_empty_v<B> == false);

struct C { static int m; };
static_assert(std::is_empty_v<C> == true);

struct D { virtual ~D(); };
static_assert(std::is_empty_v<D> == false);

union E {};
static_assert(std::is_empty_v<E> == false);

struct F { [[no_unique_address]] E e; };

struct G
{
    int:0;
    // C++ standard allow "as a special case, an unnamed bit-field with a width of zero
    // specifies alignment of the next bit-field at an allocation unit boundary.
    // Only when declaring an unnamed bit-field may the width be zero."
};
static_assert(std::is_empty_v<G>); // holds only unnamed bit-fields of zero width

int main()
{
    std::cout << std::boolalpha;
    std::cout << "F: " << std::is_empty_v<F> << '\n'; // the result is ABI-dependent
}
```

Possible output:

```text
F: true
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2015 | C++11 | the behavior was undefined if `T` is an incomplete union
      type | the base characteristic is `std::false_type` in this case

### See also

- **is_class (C++11)** — checks if a type is a non-union class type (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/types/is_empty*
