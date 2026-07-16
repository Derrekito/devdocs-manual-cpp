# std::variant_alternative, std::variant_alternative_t

```cpp
template <std::size_t I, class T>
struct variant_alternative; /* undefined */  // (1) (since C++17)
template <std::size_t I, class... Types>
struct variant_alternative<I, variant<Types...>>;  // (2) (since C++17)
template <std::size_t I, class T> class variant_alternative<I, const T>;  // (3) (since C++17)
template <std::size_t I, class T>
class variant_alternative<I, volatile T>;
template <std::size_t I, class T>
class variant_alternative<I, const volatile T>;  // (3) (since C++17) (deprecated in C++20)
```

Provides compile-time indexed access to the types of the alternatives of the
possibly cv-qualified variant, combining cv-qualifications of the variant (if
any) with the cv-qualifications of the alternative.

Formally,

2) meets the TransformationTrait requirements with a member typedef `type` equal
   to the type of the alternative with index `I`

3) meets the TransformationTrait requirements with a member typedef `type` that
   names, respectively, `std::add_const_t<std::variant_alternative_t<I,T>>`,
   `std::add_volatile_t<std::variant_alternative_t<I,T>>`, and
   `std::add_cv_t<std::variant_alternative_t<I,T>>`

### Member types

- **type** — the type of `I`th alternative of the variant, where `I` must be in
  `[0, sizeof...(Types))`, otherwise the program is ill-formed.

### Helper template alias

```cpp
template <size_t I, class T>
using variant_alternative_t = typename variant_alternative<I, T>::type;  // (since C++17)
```

### Example

```cpp
#include <variant>
#include <iostream>

using my_variant = std::variant<int, float>;
static_assert(std::is_same_v
    <int,   std::variant_alternative_t<0, my_variant>>);
static_assert(std::is_same_v
    <float, std::variant_alternative_t<1, my_variant>>);
// cv-qualification on the variant type propagates to the extracted alternative type.
static_assert(std::is_same_v
    <const int, std::variant_alternative_t<0, const my_variant>>);

int main()
{
    std::cout << "All static assertions passed.\n";
}
```

Output:

```text
All static assertions passed.
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2974 | C++17 | out-of-bounds index resulted in undefined behavior | made
      ill-formed

### See also

- **variant_sizevariant_size_v (C++17)** — obtains the size of the variant's
  list of alternatives at compile time (class template) (variable template)
- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/utility/variant/variant_alternative*
