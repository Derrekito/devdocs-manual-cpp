# std::any::operator=

```cpp
any& operator=( const any& rhs );  // (1) (since C++17)
any& operator=( any&& rhs ) noexcept;  // (2) (since C++17)
template< typename ValueType >
    any& operator=( ValueType&& rhs );  // (3) (since C++17)
```

Assigns contents to the contained value.

1) Assigns by copying the state of `rhs`, as if by `any(rhs).swap(*this)`.

2) Assigns by moving the state of `rhs`, as if by
   `any(std::move(rhs)).swap(*this)`. `rhs` is left in a valid but unspecified
   state after the assignment.

3) Assigns the type and value of `rhs`, as if by
   `any(std::forward<ValueType>(rhs)).swap(*this)`. This overload participates
   in overload resolution only if `std::decay_t<ValueType>` is not the same type
   as `any` and `std::is_copy_constructible_v<std::decay_t<ValueType>>` is
   `true`.

### Template parameters

- **ValueType** — contained value type

**Type requirements**

**-`std::decay_t<ValueType>` must meet the requirements of CopyConstructible.**

### Parameters

- **rhs** — object whose contained value to assign

### Return value

`*this`

### Exceptions

1,3) Throws `std::bad_alloc` or any exception thrown by the constructor of the
   contained type. If an exception is thrown, there are no effects (strong
   exception guarantee).

### Example

### See also

- **(constructor)** — constructs an `any` object (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/any/operator%3D*
