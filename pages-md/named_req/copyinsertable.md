# C++ named requirements: CopyInsertable (since C++11)

Specifies that an instance of the type can be copy-constructed in-place by a
given allocator.

### Requirements

The type `T` is **CopyInsertable** into the container `X` whose `value_type` is
identical to `T` if `T` is MoveInsertable into `X`, and, given

- **`A`** — an allocator type
- **`m`** — an lvalue of type `A`
- **`p`** — the pointer of type `T*` prepared by the container
- **`v`** — expression of type (possibly `const`) `T`

where `X::allocator_type` is identical to
`std::allocator_traits<A>::rebind_alloc<T>`,

the following expression is well-formed:

```cpp
std::allocator_traits<A>::construct(m, p, v);
```

And after evaluation, the value of `*p` is equivalent to the value of `v`. The
value of `v` is unchanged.

If `X` is not allocator-aware or is a `std::basic_string` specialization, the
term is defined as if `A` were `std::allocator<T>`, except that no allocator
object needs to be created, and user-defined specializations of `std::allocator`
are not instantiated.

### Notes

If `A` is `std::allocator<T>`, then this will call placement-new, as by
`::new((void*)p) T(v)`(until C++20)`std::construct_at(p, v)`(since C++20).

Although it is required that customized `construct` is used when constructing
elements of `std::basic_string` until C++23, all implementations only used the
default mechanism. The requirement is corrected by P1072R10 to match existing
practice.

---
*Source: https://en.cppreference.com/w/cpp/named_req/CopyInsertable*
