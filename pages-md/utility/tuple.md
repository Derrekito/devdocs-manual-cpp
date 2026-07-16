# std::tuple

```cpp
template< class... Types >
class tuple;  // (since C++11)
```

Class template `std::tuple` is a fixed-size collection of heterogeneous values.
It is a generalization of `std::pair`.

If `std::is_trivially_destructible<Ti>::value` is `true` for every `Ti` in
`Types`, the destructor of `tuple` is trivial.

### Template parameters

- **Types...** — the types of the elements that the tuple stores. Empty list is
  supported.

### Member functions

- **(constructor)** — constructs a new `tuple` (public member function)
- **operator=** — assigns the contents of one `tuple` to another (public member
  function)
- **swap** — swaps the contents of two `tuple`s (public member function)

### Non-member functions

- **make_tuple (C++11)** — creates a `tuple` object of the type defined by the
  argument types (function template)
- **tie (C++11)** — creates a `tuple` of lvalue references or unpacks a tuple
  into individual objects (function template)
- **forward_as_tuple (C++11)** — creates a `tuple` of forwarding references
  (function template)
- **tuple_cat (C++11)** — creates a `tuple` by concatenating any number of
  tuples (function template)
- **get(std::tuple) (C++11)** — tuple accesses specified element (function
  template)
- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (removed in C++20)(removed in C++20)(removed in C++20)(removed in
  C++20)(removed in C++20)(C++20)** — lexicographically compares the values in
  the tuple (function template)
- **std::swap(std::tuple) (C++11)** — specializes the `std::swap` algorithm
  (function template)

### Helper concepts

- ***tuple-likepair-like* (C++23)** — specifies that a type implemented the
  *tuple protocol* (`std::get`, `std::tuple_element`, `std::tuple_size`)
  (exposition-only concept*)

### Helper classes

- **std::tuple_size<std::tuple> (C++11)** — obtains the size of `tuple` at
  compile time (class template specialization)
- **std::tuple_element<std::tuple> (C++11)** — obtains the type of the specified
  element (class template specialization)
- **std::uses_allocator<std::tuple> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::basic_common_reference<*tuple-like*> (C++23)** — determines the common
  reference type of a `tuple` and a `tuple-like` type (class template
  specialization)
- **std::common_type<*tuple-like*> (C++23)** — determines the common type of a
  `tuple` and a `tuple-like` type (class template specialization)
- **std::formatter<*pair-or-tuple*> (C++23)** — formatting support for `pair`
  and `tuple` (class template specialization)
- **ignore (C++11)** — placeholder to skip an element when unpacking a `tuple`
  using `tie` (constant)

### Deduction guides (since C++17)

### Notes

- Since the "shape" of a tuple -- its size, the types of its elements, and the
  ordering of those types -- are part of its type signature, they must all be
  available at compile time and can only depend on other compile-time
  information. This means that many conditional operations on tuples -- in
  particular, conditional prepend/append and filter -- are only possible if the
  conditions can be evaluated at compile time. For example, given a
  `std::tuple<int, double, int>`, it is possible to filter on types -- e.g.
  returning a `std::tuple<int, int>` -- but not to filter on whether or not each
  element is positive (which would have a different type signature depending on
  runtime values of the tuple), unless all the elements were themselves
  `constexpr`. As a workaround, one can work with tuples of `std::optional`, but
  there is still no way to adjust the size based on runtime information.
- Until N4387 (applied as a defect report for C++11), a function could not
  return a tuple using copy-list-initialization:

```cpp
std::tuple<int, int> foo_tuple()
{
    return {1, -1};  // Error until N4387
    return std::tuple<int, int>{1, -1}; // Always works
    return std::make_tuple(1, -1); // Always works
}
```

### Example

```cpp
#include <iostream>
#include <stdexcept>
#include <string>
#include <tuple>

std::tuple<double, char, std::string> get_student(int id)
{
    switch (id)
    {
        case 0: return {3.8, 'A', "Lisa Simpson"};
        case 1: return {2.9, 'C', "Milhouse Van Houten"};
        case 2: return {1.7, 'D', "Ralph Wiggum"};
        case 3: return {0.6, 'F', "Bart Simpson"};
    }

    throw std::invalid_argument("id");
}

int main()
{
    const auto student0 = get_student(0);
    std::cout << "ID: 0, "
              << "GPA: " << std::get<0>(student0) << ", "
              << "grade: " << std::get<1>(student0) << ", "
              << "name: " << std::get<2>(student0) << '\n';

    const auto student1 = get_student(1);
    std::cout << "ID: 1, "
              << "GPA: " << std::get<double>(student1) << ", "
              << "grade: " << std::get<char>(student1) << ", "
              << "name: " << std::get<std::string>(student1) << '\n';

    double gpa2;
    char grade2;
    std::string name2;
    std::tie(gpa2, grade2, name2) = get_student(2);
    std::cout << "ID: 2, "
              << "GPA: " << gpa2 << ", "
              << "grade: " << grade2 << ", "
              << "name: " << name2 << '\n';

    // C++17 structured binding:
    const auto [gpa3, grade3, name3] = get_student(3);
    std::cout << "ID: 3, "
              << "GPA: " << gpa3 << ", "
              << "grade: " << grade3 << ", "
              << "name: " << name3 << '\n';
}
```

Output:

```text
ID: 0, GPA: 3.8, grade: A, name: Lisa Simpson
ID: 1, GPA: 2.9, grade: C, name: Milhouse Van Houten
ID: 2, GPA: 1.7, grade: D, name: Ralph Wiggum
ID: 3, GPA: 0.6, grade: F, name: Bart Simpson
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2796 | C++11 | triviality of the destructor of `tuple` was unspecified |
      specified

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/tuple*
