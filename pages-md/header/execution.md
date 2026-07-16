# Standard library header <execution> (C++17)

This header is part of the algorithm library.

**Classes**

- **is_execution_policy (C++17)** — test whether a class represents an execution
  policy (class template)
- **sequenced_policyparallel_policyparallel_unsequenced_policyunsequenced_policy
  (C++17)(C++17)(C++17)(C++20)** — execution policy types (class)

**Constants**

- **seqparpar_unsequnseq (C++17)(C++17)(C++17)(C++20)** — global execution
  policy objects (constant)

### Synopsis

```cpp
namespace std {
  // execution policy type trait
  template<class T> struct is_execution_policy;
  template<class T>
  inline constexpr bool is_execution_policy_v = is_execution_policy<T>::value;
}

namespace std::execution {
  // sequenced execution policy
  class sequenced_policy;

  // parallel execution policy
  class parallel_policy;

  // parallel and unsequenced execution policy
  class parallel_unsequenced_policy;

  // unsequenced execution policy
  class unsequenced_policy;

  // execution policy objects
  inline constexpr sequenced_policy            seq{ /*unspecified*/ };
  inline constexpr parallel_policy             par{ /*unspecified*/ };
  inline constexpr parallel_unsequenced_policy par_unseq{ /*unspecified*/ };
  inline constexpr unsequenced_policy          unseq{ /*unspecified*/ };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/execution*
