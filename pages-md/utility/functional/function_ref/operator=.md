# std::function_ref::operator=

```cpp
constexpr function_ref& operator=( const function_ref& ) noexcept = default;  // (1) (since C++26)
template< class T >
constexpr function_ref& operator=( T ) = delete;  // (2) (since C++26)
```

1) Copy assignment operator is explicitly-defaulted. `std::function_ref`
   satisfies `copyable` and TriviallyCopyable. This defaulted assignment
   operator performs a shallow copy of the stored `thunk-ptr` and
   `bound-entity`.

2) User-defined assignment operator is explicitly-deleted if `T` is not the same
   type as `std::function_ref`, `std::is_pointer_v<T>` is `false`, and `T` is
   not a specialization of `std::nontype_t`. This overload participates in
   overload resolution only if the constraints are satisfied in the conditions
   above.

### Return value

`*this`

### See also

- **(constructor) (C++26)** — constructs a new `function_ref` object (public
  member function)
- **operator=** — assigns a new target (public member function of
  `std::function<R(Args...)>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function_ref/operator%3D*
