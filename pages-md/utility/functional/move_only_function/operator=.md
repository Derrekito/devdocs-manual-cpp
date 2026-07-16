# std::move_only_function::operator=

```cpp
move_only_function& operator=( move_only_function&& other );  // (1) (since C++23)
move_only_function& operator=( const move_only_function& ) = delete;  // (2) (since C++23)
move_only_function& operator=( std::nullptr_t ) noexcept;  // (3) (since C++23)
template< class F >
move_only_function& operator=( F&& f );  // (4) (since C++23)
```

Assigns a new target to `std::move_only_function` or destroys its target.

1) Moves the target of `other` to `*this` or destroys the target of `*this` (if
   any) if `other` is empty, by `auto(std::move(other)).swap(*this)`. `other` is
   in a valid state with an unspecified value after move assignment.

2) Copy assignment operator is deleted. `std::move_only_function` does not
   satisfy CopyAssignable.

3) Destroys the current target if it exists. `*this` is empty after the call.

4) Sets the target of `*this` to the callable `f`, or destroys the current
   target if `f` is a null function pointer, a null pointer to member function,
   or an empty `std::move_only_function`, as if by executing
   `move_only_function(std::forward<F>(f)).swap(*this);`. This overload
   participates in overload resolution only if the constructor of
   `move_only_function` from `F` participates in overload resolution. The
   program is ill-formed or has undefined behavior if the selected constructor
   call is ill-formed or has undefined behavior.

### Parameters

- **other** — another `std::move_only_function` object to move the target of
- **f** — a callable object to initialize the new target with

### Return value

`*this`

### Notes

It is intentional not to require the move assignment operator to be `noexcept`
to leave room for an allocator-aware `move_only_function` in future.

`move_only_function` can be assigned from `std::in_place_type<Fn>` given it can
be constructed from that argument.

### See also

- **operator=** — assigns a new target (public member function of
  `std::function<R(Args...)>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/operator%3D*
