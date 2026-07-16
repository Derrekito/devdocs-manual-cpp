# C++ named requirements: MoveInsertable (since C++11)

Specifies that an object of the type can be constructed into uninitialized
storage from an rvalue of that type by a given allocator.

### Requirements

The type `T` is **MoveInsertable** into the container `X` whose `value_type` is
identical to `T` if, given

- **`A`** — an allocator type
- **`m`** — an lvalue of type `A`
- **`p`** — the pointer of type `T*` prepared by the container
- **`rv`** — rvalue expression of type `T`

where `X::allocator_type` is identical to
`std::allocator_traits<A>::rebind_alloc<T>`,

the following expression is well-formed:

```cpp
std::allocator_traits<A>::construct(m, p, rv);
```

And after evaluation, the value of `*p` is equivalent to the value formerly held
by `rv` (`rv` remains valid, but is in an unspecified state).

If `X` is not allocator-aware or is a `std::basic_string` specialization, the
term is defined as if `A` were `std::allocator<T>`, except that no allocator
object needs to be created, and user-defined specializations of `std::allocator`
are not instantiated.

### Notes

If `A` is `std::allocator<T>`, then this will call placement-new, as by
`::new((void*)p) T(rv)`(until C++20)`std::construct_at(p, rv)`(since C++20).
This effectively requires `T` to be move constructible.

If `std::allocator<T>` or a similar allocator is used, a class does not have to
implement a move constructor to satisfy this type requirement: a copy
constructor that takes a `const T&` argument can bind rvalue expressions. If a
MoveInsertable class implements a move constructor, it may also implement move
semantics to take advantage of the fact that the value of `rv` after
construction is unspecified.

Although it is required that customized `construct` is used when constructing
elements of `std::basic_string` until C++23, all implementations only used the
default mechanism. The requirement is corrected by P1072R10 to match existing
practice.

### See also

**CopyInsertable**

---
*Source: https://en.cppreference.com/w/cpp/named_req/MoveInsertable*
