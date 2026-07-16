# std::is_execution_policy

```cpp
template< class T >
struct is_execution_policy;  // (since C++17)
```

Checks whether `T` is a standard or implementation-defined execution policy
type.

Provides the member constant `value` which is equal to `true`, if `T` is a
standard execution policy type, or an implementation-defined execution policy
type. Otherwise, `value` is equal to `false`.

The behavior of a program that adds specializations for
`std::is_execution_policy` or `std::is_execution_policy_v` is undefined.

### Template parameters

- **T** — a type to check

### Helper template

```cpp
template< class T >
inline constexpr bool is_execution_policy_v = std::is_execution_policy<T>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` is a standard or implementation-defined
  execution policy type, `false` otherwise (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### See also

- **sequenced_policyparallel_policyparallel_unsequenced_policyunsequenced_policy
  (C++17)(C++17)(C++17)(C++20)** — execution policy types (class)
- **seqparpar_unsequnseq (C++17)(C++17)(C++17)(C++20)** — global execution
  policy objects (constant)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/is_execution_policy*
