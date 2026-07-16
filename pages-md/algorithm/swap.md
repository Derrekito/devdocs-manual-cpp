# std::swap

```cpp
template< class T >
void swap( T& a, T& b );  // (until C++11)
template< class T >
void swap( T& a, T& b ) noexcept( /* see below */ );  // (since C++11) (until C++20)
template< class T >
constexpr void swap( T& a, T& b ) noexcept( /* see below */ );  // (since C++20)
template< class T2, std::size_t N >
void swap( T2 (&a)[N], T2 (&b)[N] );  // (until C++11)
template< class T2, std::size_t N >
void swap( T2 (&a)[N], T2 (&b)[N] ) noexcept( /* see below */ );  // (since C++11) (until C++20)
template< class T2, std::size_t N >
constexpr void swap( T2 (&a)[N], T2 (&b)[N] ) noexcept( /* see below */ );  // (since C++20)
```

Exchanges the given values.

1) Swaps the values `a` and `b`. This overload does not participate in overload
   resolution unless `std::is_move_constructible_v<T> &&
   std::is_move_assignable_v<T>` is `true`.(since C++17)

2) Swaps the arrays `a` and `b`. In effect calls `std::swap_ranges(a, a + N,
   b)`. This overload does not participate in overload resolution unless
   `std::is_swappable_v<T2>` is `true`.(since C++17)

### Parameters

- **a, b** — the values to be swapped

**Type requirements**

**-`T` must meet the requirements of CopyConstructible and CopyAssignable(until C++11)MoveConstructible and MoveAssignable(since C++11)**

**-`T2` must meet the requirements of Swappable.**

### Return value

(none)

### Exceptions

1) (none) (until C++11) `noexcept` specification: `noexcept(
   std::is_nothrow_move_constructible<T>::value &&
   std::is_nothrow_move_assignable<T>::value )` (since C++11)

2) `noexcept` specification: `noexcept(noexcept(swap(*a, *b)))` The lookup for
   the identifier `swap` in the exception specification finds this function
   template in addition to anything found by the usual lookup rules, making the
   exception specification equivalent to C++17 `std::is_nothrow_swappable`.
   (since C++11) (until C++17) `noexcept` specification:
   `noexcept(std::is_nothrow_swappable_v<T2>)` (since C++17)

### Complexity

1) Constant

2) Linear in `N`

### Specializations

`std::swap` may be specialized in namespace std for program-defined types, but
such specializations are not found by ADL (the namespace std is not the
associated namespace for the program-defined type).
*(until C++20)*

The expected way to make a program-defined type swappable is to provide a
non-member function swap in the same namespace as the type: see Swappable for
details.

The following overloads are already provided by the standard library:

- **std::swap(std::pair) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::tuple) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::shared_ptr) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::weak_ptr) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::unique_ptr) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::function) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::basic_string)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::array) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::deque)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::forward_list) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::list)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::vector)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::map)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::multimap)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::set)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::multiset)** — specializes the `std::swap` algorithm (function
  template)
- **std::swap(std::unordered_map) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::unordered_multimap) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::unordered_set) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::unordered_multiset) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::queue) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::priority_queue) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::stack) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::valarray) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::basic_stringbuf) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_istringstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_ostringstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_stringstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_filebuf) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_ifstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_ofstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_fstream) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_syncbuf) (C++20)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_spanbuf) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_ispanstream) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_ospanstream) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_spanstream) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::basic_regex) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::match_results) (C++11)** — specializes the **`std::swap`**
  algorithm (function template)
- **std::swap(std::thread) (C++11)** — specializes the `std::swap` algorithm
  (function)
- **std::swap(std::unique_lock) (C++11)** — specialization of `std::swap` for
  `unique_lock` (function template)
- **std::swap(std::shared_lock) (C++14)** — specialization of `std::swap` for
  `shared_lock` (function template)
- **std::swap(std::promise) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::packaged_task) (C++11)** — specializes the `std::swap`
  algorithm (function template)
- **std::swap(std::optional) (C++17)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::any) (C++17)** — specializes the `std::swap` algorithm
  (function)
- **std::swap(std::variant) (C++17)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::basic_stacktrace) (C++23)** — specializes the `std::swap`
  algorithm (function template)
- **swap(std::filesystem::path) (C++17)** — swaps two paths (function)
- **swap(std::expected) (C++23)** — specializes the `std::swap` algorithm
  (function)
- **swap(std::jthread) (C++20)** — specializes the `std::swap` algorithm
  (function)
- **swap(std::move_only_function) (C++23)** — overloads the `std::swap`
  algorithm (function)
- **swap(std::stop_source) (C++20)** — specializes the `std::swap` algorithm
  (function)
- **swap(std::stop_token) (C++20)** — specializes the `std::swap` algorithm
  (function)

### Example

```cpp
#include <algorithm>
#include <iostream>

namespace Ns
{
    class A
    {
        int id {};

        friend void swap(A& lhs, A& rhs)
        {
            std::cout << "swap(" << lhs << ", " << rhs << ")\n";
            std::swap(lhs.id, rhs.id);
        }

        friend std::ostream& operator<<(std::ostream& os, A const& a)
        {
            return os << "A::id=" << a.id;
        }

    public:
        A(int i) : id {i} {}
        A(A const&) = delete;
        A& operator = (A const&) = delete;
    };
}

int main()
{
    int a = 5, b = 3;
    std::cout << a << ' ' << b << '\n';
    std::swap(a, b);
    std::cout << a << ' ' << b << '\n';

    Ns::A p {6}, q {9};
    std::cout << p << ' ' << q << '\n';
//  std::swap(p, q); // error, type requirements are not satisfied
    swap(p, q);      // OK, ADL finds the appropriate friend `swap`
    std::cout << p << ' ' << q << '\n';
}
```

Output:

```text
5 3
3 5
A::id=6 A::id=9
swap(A::id=6, A::id=9)
A::id=9 A::id=6
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 227 | C++98 | `T` was not required to be CopyConstructible or
      DefaultConstructible (a temporary object of type `T` might not be able to
      be constructed) | `T` is also required to be CopyConstructible
  LWG 809 | C++98 | arrays could not be swapped | added overload (2)
  LWG 2554 | C++11 | swapping multi-dimensional arrays can never be `noexcept`
      due to name lookup problems | made to work

### See also

- **ranges::swap (C++20)** — swaps the values of two objects (customization
  point object)
- **iter_swap** — swaps the elements pointed to by two iterators (function
  template)
- **swap_ranges** — swaps two ranges of elements (function template)
- **exchange (C++14)** — replaces the argument with a new value and returns its
  previous value (function template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/swap*
