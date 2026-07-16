# std::allocator<T>::allocate

```cpp
pointer allocate( size_type n, const void* hint = 0 );  // (until C++17)
T* allocate( std::size_t n, const void* hint );  // (since C++17) (deprecated) (removed in C++20)
T* allocate( std::size_t n );  // (since C++17) (until C++20)
[[nodiscard]] constexpr T* allocate( std::size_t n );  // (since C++20)
```

Allocates `n * sizeof(T)` bytes of uninitialized storage by calling `::operator
new(std::size_t)` or `::operator new(std::size_t, std::align_val_t)`(since
C++17), but it is unspecified when and how this function is called. The pointer
`hint` may be used to provide locality of reference: the allocator, if supported
by the implementation, will attempt to allocate the new memory block as close as
possible to `hint`.

Then, this function creates an array of type `T[n]` in the storage and starts
its lifetime, but does not start lifetime of any of its elements.

Use of this function is ill-formed if `T` is an incomplete type.

In order to use this function in a constant expression, the allocated storage
must be deallocated within the evaluation of the same expression.
*(since C++20)*

### Parameters

- **n** — the number of objects to allocate storage for
- **hint** — pointer to a nearby memory location

### Return value

Pointer to the first element of an array of `n` objects of type `T` whose
elements have not been constructed yet.

### Exceptions

Throws `std::bad_array_new_length` if `std::numeric_limits<std::size_t>::max() /
sizeof(T) < n`.
*(since C++11)*

Throws `std::bad_alloc` if allocation fails.

### Notes

The "unspecified when and how" wording makes it possible to combine or optimize
away heap allocations made by the standard library containers, even though such
optimizations are disallowed for direct calls to `::operator new`. For example,
this is implemented by libc++ ([1] and [2]).

After calling `allocate()` and before construction of elements, pointer
arithmetic of `T*` is well-defined within the allocated array, but the behavior
is undefined if elements are accessed.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 578 | C++98 | `hint` was required to be either `​0​` or a pointer
      previously returned from `allocate()` and not yet passed to `deallocate()`
      | not required
  LWG 3190 | C++11 | `allocate()` might allocate storage of wrong size | throws
      `std::bad_array_new_length` instead

### See also

- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/allocate*
