# std::assume_aligned

```cpp
template< std::size_t N, class T >
[[nodiscard]] constexpr T* assume_aligned( T* ptr );  // (since C++20)
```

Informs the implementation that the object `ptr` points to is aligned to at
least `N`. The implementation may use this information to generate more
efficient code, but it might only make this assumption if the object is accessed
via the return value of `assume_aligned`.

`N` must be a power of 2. The behavior is undefined if `ptr` does not point to
an object of type `T` (ignoring cv-qualification at every level), or if the
object's alignment is not at least `N`.

### Return value

`ptr`.

### Exceptions

Throws nothing.

### Notes

To ensure that the program benefits from the optimizations enabled by
`assume_aligned`, it is important to access the object via its return value:

```cpp
void f(int* p)
{
    int* p1 = std::assume_aligned<256>(p);
    // Use p1, not p, to ensure benefit from the alignment assumption.
    // However, the program has undefined behavior if p is not aligned
    // regardless of whether p1 is used.
}
```

It is up to the program to ensure that the alignment assumption actually holds.
A call to `assume_aligned` does not cause the compiler to verify or enforce
this.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_assume_aligned` | 201811L | (C++20) | `std::assume_aligned`

### Example

### See also

- **`alignof` operator(C++11)** — queries alignment requirements of a type
- **`alignas` specifier(C++11)** — specifies that the storage for the variable
  should be aligned by specific amount
- **aligned_storage (C++11)(deprecated in C++23)** — defines the type suitable
  for use as uninitialized storage for types of given size (class template)
- **align (C++11)** — aligns a pointer in a buffer (function)
- **`[[assume(expression)]]`(C++23)** — specifies that the *expression* will
  always evaluate to true at a given point (attribute specifier)

---
*Source: https://en.cppreference.com/w/cpp/memory/assume_aligned*
