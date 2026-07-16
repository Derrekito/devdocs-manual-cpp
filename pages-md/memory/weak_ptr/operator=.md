# std::weak_ptr<T>::operator=

```cpp
weak_ptr& operator=( const weak_ptr& r ) noexcept;  // (1) (since C++11)
template< class Y >
weak_ptr& operator=( const weak_ptr<Y>& r ) noexcept;  // (2) (since C++11)
template< class Y >
weak_ptr& operator=( const shared_ptr<Y>& r ) noexcept;  // (3) (since C++11)
weak_ptr& operator=( weak_ptr&& r ) noexcept;  // (4) (since C++11)
template< class Y >
weak_ptr& operator=( weak_ptr<Y>&& r ) noexcept;  // (5) (since C++11)
```

Replaces the managed object with the one managed by `r`. The object is shared
with `r`. If `r` manages no object, `*this` manages no object too.

1-3) Equivalent to `std::weak_ptr<T>(r).swap(*this)`.

4,5) Equivalent to `std::weak_ptr<T>(std::move(r)).swap(*this)`.

### Parameters

- **r** — smart pointer to share an object with

### Return value

`*this`

### Notes

The implementation may meet the requirements without creating a temporary
`weak_ptr` object.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2315 | C++11 | move semantic was not enabled for `weak_ptr` | enabled

### See also

- **(constructor)** — creates a new `weak_ptr` (public member function)
- **swap** — swaps the managed objects (public member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr/operator%3D*
