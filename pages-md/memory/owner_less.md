# std::owner_less

```cpp
template< class T >
struct owner_less; /* undefined */  // (since C++11) (until C++17)
template< class T = void >
struct owner_less; /* undefined */  // (since C++17)
template< class T >
struct owner_less<std::shared_ptr<T>>;  // (2) (since C++11)
template< class T >
struct owner_less<std::weak_ptr<T>>;  // (3) (since C++11)
template<>
struct owner_less<void>;  // (4) (since C++17)
```

This function object provides owner-based (as opposed to value-based) mixed-type
ordering of both `std::weak_ptr` and `std::shared_ptr`. The order is such that
two smart pointers compare equivalent only if they are both empty or if they
share ownership, even if the values of the raw pointers obtained by `get()` are
different (e.g. because they point at different subobjects within the same
object).

1) Owner-based mixed-type ordering is not provided for types other than
   `std::shared_ptr` and `std::weak_ptr`.

2) The owner-based mixed-type ordering of `std::shared_ptr`.

It is the preferred comparison predicate when building associative containers
   with `std::shared_ptr` as keys, that is, `std::map<std::shared_ptr<T>, U,
   std::owner_less<std::shared_ptr<T>>>`.

3) The owner-based mixed-type ordering of `std::weak_ptr`.

It is the preferred comparison predicate when building associative containers
   with `std::weak_ptr` as keys, that is, `std::map<std::weak_ptr<T>, U,
   std::owner_less<std::weak_ptr<T>>>`.

4) The `void` specialization deduces the parameter types from the arguments.

The default operator< is not defined for weak pointers, and may wrongly consider
two shared pointers for the same object non-equivalent (see
`std::shared_ptr::owner_before`).

### Specializations
The standard library provides a specialization of `std::owner_less` when `T` is
not specified. In this case, the parameter types are deduced from the arguments
(each of which must still be either a `std::shared_ptr` or a `std::weak_ptr`).
- **owner_less<void>** — function object providing mixed-type owner-based
  ordering of shared and weak pointers, regardless of the type of the pointee
  (class template specialization)
*(since C++17)*

### Nested types
- **`result_type` (deprecated in C++17)** — (2,3) bool
- **`first_argument_type` (deprecated in C++17)** — (2) std::shared_ptr<T> (3)
  std::weak_ptr<T>
- **`second_argument_type` (deprecated in C++17)** — (2) std::shared_ptr<T> (3)
  std::weak_ptr<T>
*(until C++20)*

### Member functions

- ****operator()**** — compares its arguments using owner-based semantics
  (function)

## std::owner_less::operator()

```cpp
member only of specialization (2)
bool operator()( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<T>& rhs ) const noexcept;  // (since C++11)
member only of specialization (3)
bool operator()( const std::weak_ptr<T>& lhs,
                 const std::weak_ptr<T>& rhs ) const noexcept;  // (since C++11)
member of both template specializations
bool operator()( const std::shared_ptr<T>& lhs,
                 const std::weak_ptr<T>& rhs ) const noexcept;  // (since C++11)
bool operator()( const std::weak_ptr<T>& lhs,
                 const std::shared_ptr<T>& rhs ) const noexcept;  // (since C++11)
```

Compares `lhs` and `rhs` using owner-based semantics. Effectively calls
`lhs.owner_before(rhs)`.

The ordering is strict weak ordering relation.

`lhs` and `rhs` are equivalent only if they are both empty or share ownership.

### Parameters

- **lhs, rhs** — shared-ownership pointers to compare

### Return value

`true` if `lhs` is *less than* `rhs` as determined by the owner-based ordering,
`false` otherwise.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2873 | C++11 | operator() was not required to be noexcept | required to be
      noexcept

### See also

- **owner_before** — provides owner-based ordering of shared pointers (public
  member function of `std::shared_ptr<T>`)
- **owner_before** — provides owner-based ordering of weak pointers (public
  member function of `std::weak_ptr<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/owner_less*
