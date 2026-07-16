# std::function_ref

```cpp
template< class... >
class function_ref; // not defined  // (1) (since C++26)
template< class R, class... Args >
class function_ref<R(Args...)>;
template< class R, class... Args >
class function_ref<R(Args...) noexcept>;
template< class R, class... Args >
class function_ref<R(Args...) const>;
template< class R, class... Args >
class function_ref<R(Args...) const noexcept>;  // (2) (since C++26)
```

Class template `std::function_ref` is a non-owning function wrapper.
`std::function_ref` objects can store and invoke reference to Callable *target*
- functions, lambda expressions, bind expressions, or other function objects,
but not pointers to member functions and pointers to member objects.
`std::nontype` can be used to construct `std::function_ref` by passing function
pointers, pointers to member functions, and pointers to member objects.

`std::function_ref`s supports every possible combination of cv-qualifiers, and
noexcept-specifiers not including `volatile` provided in its template parameter.

Every specialization of `std::function_ref` is a TriviallyCopyable type that
satisfies `copyable`.

### Member objects

- **`bound-entity` (private)** — an object that has an unspecified
  TriviallyCopyable type `BoundEntityType`, that satisfies `copyable` and is
  capable of storing a pointer to object value or pointer to function value
  (exposition-only member object*)
- **`thunk-ptr` (private)** — a pointer to function of type
  `R(*)(BoundEntityType, Args&&...) noexcept(/*noex*/)` where `/*noex*/` is true
  if noexcept is present in function signature as part of the template parameter
  of `std::function_ref` (exposition-only member object*)

### Member functions

- **(constructor) (C++26)** — constructs a new `function_ref` object (public
  member function)
- **operator= (C++26)** — assigns a `function_ref` (public member function)
- **operator() (C++26)** — invokes the stored thunk of a `function_ref` (public
  member function)

### Deduction guides

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_function_ref` | 202306L | (C++26) | `std::function_ref`

### Example

### See also

- **function (C++11)** — wraps callable object of any copy constructible type
  with specified function call signature (class template)
- **copyable_function (C++26)** — refinement of `std::move_only_function` that
  wraps callable object of any copy constructible type (class template)
- **move_only_function (C++23)** — wraps callable object of any type with
  specified function call signature (class template)
- **nontype nontype_t (C++26)** — value construction tag (tag)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function_ref*
