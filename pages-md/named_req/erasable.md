# C++ named requirements: Erasable (since C++11)

Specifies that an object of the type can be destroyed by a given Allocator.

### Requirements

The type `T` is **Erasable** from the Container `X` whose `value_type` is
identical to `T` if, given

- **`A`** — an allocator type
- **`m`** — an lvalue of type `A`
- **`p`** — the pointer of type `T*` prepared by the container

where `X::allocator_type` is identical to
`std::allocator_traits<A>::rebind_alloc<T>`,

the following expression is well-formed:

```cpp
std::allocator_traits<A>::destroy(m, p);
```

If `X` is not allocator-aware or is a `std::basic_string` specialization, the
term is defined as if `A` were `std::allocator<T>`, except that no allocator
object needs to be created, and user-defined specializations of `std::allocator`
are not instantiated.

### Notes

All standard library containers require that their element type satisfies
Erasable.

With the default allocator, this requirement is equivalent to the validity of
`p->~T()`, which accepts class types with accessible destructors and all scalar
types, but rejects array types, function types, reference types, and void.
*(until C++20)*

With the default allocator, this requirement is equivalent to the validity of
`std::destroy_at(p)`, which accepts class types with accessible destructors and
all scalar types, as well as arrays thereof.
*(since C++20)*

Although it is required that customized `destroy` is used when destroying
elements of `std::basic_string` until C++23, all implementations only used the
default mechanism. The requirement is corrected by P1072R10 to match existing
practice.

### See also

**CopyInsertable**

**MoveInsertable**

**EmplaceConstructible**

**Destructible**

---
*Source: https://en.cppreference.com/w/cpp/named_req/Erasable*
