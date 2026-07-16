# C++ named requirements: DefaultInsertable (since C++11)

Specifies that an instance of the type can be default-constructed in-place by a
given allocator.

### Requirements

The type `T` is **DefaultInsertable** into the Container `X` whose `value_type`
is identical to `T` if, given

- **`A`** — an allocator type
- **`m`** — an lvalue of type `A`
- **`p`** — the pointer of type `T*` prepared by the container

where `X::allocator_type` is identical to
`std::allocator_traits<A>::rebind_alloc<T>`,

the following expression is well-formed:

```cpp
std::allocator_traits<A>::construct(m, p);
```

If `X` is not allocator-aware or is a `std::basic_string` specialization, the
term is defined as if `A` were `std::allocator<T>`, except that no allocator
object needs to be created, and user-defined specializations of `std::allocator`
are not instantiated.

### Notes

By default, this will value-initialize the object, as by `::new((void*)p)
T()`(until C++20)`std::construct_at(p)`(since C++20). If value-initialization is
undesirable, for example, if the object is of non-class type and zeroing out is
not needed, it can be avoided by providing a custom `Allocator::construct`.

Although it is required that customized `construct` is used when constructing
elements of `std::basic_string` until C++23, all implementations only used the
default mechanism. The requirement is corrected by P1072R10 to match existing
practice.

### See also

**DefaultConstructible**

**CopyInsertable**

**MoveInsertable**

**EmplaceConstructible**

**Erasable**

---
*Source: https://en.cppreference.com/w/cpp/named_req/DefaultInsertable*
