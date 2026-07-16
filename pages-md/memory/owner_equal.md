# std::owner_equal

```cpp
struct owner_equal;  // (since C++26)
```

This function object provides owner-based (as opposed to value-based) mixed-type
equal comparison of both `std::weak_ptr` and `std::shared_ptr`. The comparison
is such that two smart pointers compare equivalent only if they are both empty
or if they share ownership, even if the values of the raw pointers obtained by
`get()` are different (e.g. because they point at different subobjects within
the same object).

1) Owner-based mixed-type equal comparison is not provided for types other than
   `std::shared_ptr` and `std::weak_ptr`.

2) The owner-based mixed-type equal comparison of `std::shared_ptr` and
   `std::weak_ptr`.

It is the preferred comparison predicate when building unordered associative
   containers with `std::shared_ptr` and `std::weak_ptr` as keys together with
   `std::owner_hash`, that is, `std::unordered_map<std::shared_ptr<T>, U,
   std::owner_hash, std::owner_equal>` or `std::unordered_map<std::weak_ptr<T>,
   U, std::owner_hash, std::owner_equal>`.

3) `std::owner_equal` deduces the parameter types from the arguments.

### Nested types

- **`is_transparent`** — unspecified

### Member functions

- ****operator()**** — compares its arguments using owner-based semantics
  (function)

## std::owner_equal::operator()

```cpp
template< class T, class U >
bool operator()( const std::shared_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) const noexcept;  // (since C++26)
template< class T, class U >
bool operator()( const std::shared_ptr<T>& lhs,
                 const std::weak_ptr<U>& rhs ) const noexcept;  // (since C++26)
template< class T, class U >
bool operator()( const std::weak_ptr<T>& lhs,
                 const std::shared_ptr<U>& rhs ) const noexcept;  // (since C++26)
template< class T, class U >
bool operator()( const std::weak_ptr<T>& lhs,
                 const std::weak_ptr<U>& rhs ) const noexcept;  // (since C++26)
```

Compares `lhs` and `rhs` using owner-based semantics. Effectively calls
`lhs.owner_equal(rhs)`.

The equal comparison is an equivalence relation.

`lhs` and `rhs` are equivalent only if they are both empty or share ownership.

### Parameters

- **lhs, rhs** — shared-ownership pointers to compare

### Return value

`true` if `lhs` and `rhs` are both empty or share ownership as determined by the
owner-based equal comparison, `false` otherwise.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_smart_ptr_owner_equality` | 202306L | (C++26) | Enabling the use of
      `std::shared_ptr` and `std::weak_ptr` as keys in unordered associative
      containers

### See also

- **owner_equal (C++26)** — provides owner-based equal comparison of shared
  pointers (public member function of `std::shared_ptr<T>`)
- **owner_equal (C++26)** — provides owner-based equal comparison of weak
  pointers (public member function of `std::weak_ptr<T>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/owner_equal*
