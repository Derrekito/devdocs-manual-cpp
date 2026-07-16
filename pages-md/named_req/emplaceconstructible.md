# C++ named requirements: EmplaceConstructible (since C++11)

Specifies that an object of the type can be constructed from a given set of
arguments in uninitialized storage by a given allocator.

### Requirements

The type `T` is **EmplaceConstructible** into the Container `X` (whose
`value_type` is identical to `T`) from the arguments `args` if, given

- **`A`** — an allocator type
- **`m`** — an lvalue of type `A`
- **`p`** — the pointer of type `T*` prepared by the container
- **`args`** — zero or more arguments

where `X::allocator_type` is identical to
`std::allocator_traits<A>::rebind_alloc<T>`,

the following expression is well-formed:

```cpp
std::allocator_traits<A>::construct(m, p, args);
```

If `X` is not allocator-aware or is a `std::basic_string` specialization, the
term is defined as if `A` were `std::allocator<T>`, except that no allocator
object needs to be created, and user-defined specializations of `std::allocator`
are not instantiated.

### Notes

Although it is required that customized `construct` is used when constructing
elements of `std::basic_string` until C++23, all implementations only used the
default mechanism. The requirement is corrected by P1072R10 to match existing
practice.

### See also

**CopyInsertable**

**MoveInsertable**

---
*Source: https://en.cppreference.com/w/cpp/named_req/EmplaceConstructible*
