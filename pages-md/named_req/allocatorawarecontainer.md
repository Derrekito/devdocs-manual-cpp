# C++ named requirements: AllocatorAwareContainer (since C++11)

An **AllocatorAwareContainer** is a Container that holds an instance of an
Allocator and uses that instance in all its member functions to allocate and
deallocate memory and to construct and destroy objects in that memory (such
objects may be container elements, nodes, or, for unordered containers, bucket
arrays), except that `std::basic_string` specializations do not use the
allocators for construction/destruction of their elements(since C++23).

The following rules apply to container construction:

- Copy constructors of AllocatorAwareContainers obtain their instances of the
  allocator by calling
  `std::allocator_traits<allocator_type>::select_on_container_copy_construction`
  on the allocator of the container being copied.
- Move constructors obtain their instances of allocators by move-constructing
  from the allocator belonging to the old container.
- All other constructors take a `const allocator_type&` parameter.

The only way to replace an allocator is copy-assignment, move-assignment, and
swap:

- Copy-assignment will replace the allocator only if
  `std::allocator_traits<allocator_type>::propagate_on_container_copy_assignment::value`
  is `true`.
- Move-assignment will replace the allocator only if
  `std::allocator_traits<allocator_type>::propagate_on_container_move_assignment::value`
  is `true`.
- Swap will replace the allocator only if
  `std::allocator_traits<allocator_type>::propagate_on_container_swap::value` is
  `true`. Specifically, it will exchange the allocator instances through an
  unqualified call to the non-member function swap, see Swappable.

Note: swapping two containers with unequal allocators if
`propagate_on_container_swap` is `false` is undefined behavior.

- The accessor `get_allocator()` obtains a copy of the allocator that was used
  to construct the container or installed by the most recent allocator
  replacement operation.

### Requirements

#### Legend

- **`X`** — Container type
- **`T`** — Element type
- **`A`** — Allocator for `T`
- **`a`, `b`** — Objects of type `X` (non-const lvalue)
- **`t`** — Object of type `X` (lvalue or const rvalue)
- **`rv`** — Object of type `X` (non-const rvalue)
- **`m`** — Object of type `A`

#### Types

  Expression | Return type | Requirements
  `allocator_type` | `A` | `allocator_type::value_type` must be the same as
      `X::value_type`

#### Member functions and operators

  Expression | Return type | Pre/requirements | Post/effects | Complexity
  `get_allocator()` | `A` | Constant
  `X u;` | `A` is DefaultConstructible | `u.empty() == true && u.get_allocator()
      == A()` | Constant
  `X u(m);` | `u.empty() == true && u.get_allocator() == m` | Constant
  `X u(t,m);` | `T` is CopyInsertable into `X` | `u == t && u.get_allocator() ==
      m` | Linear
  `X u(rv);` | Move constructor of `A` must not throw exceptions | `u` has the
      same elements and an equal allocator as `rv` had before the construction |
      Constant
  `X u(rv,m);` | `T` is MoveInsertable into `X` | The elements of `u` are the
      same or copies of those of `rv` and `u.get_allocator() == m` | Constant if
      `m == rv.get_allocator()`, otherwise linear
  `a = t` | `X&` | `T` is CopyInsertable into `X` and CopyAssignable | `a == t`
      | Linear
  `a = rv` | `X&` | If the allocator will *not* be replaced by move-assignment
      (see above), then `T` is MoveInsertable into `X` and MoveAssignable | All
      existing elements of `a` are either move assigned to or destroyed; if `a`
      and `rv` do not refer the same object, `a` is equal to the value that `rv`
      had before the assignment | Linear
  `a.swap(b)` | `void` | Exchanges the contents of `a` and `b` | Constant

### Notes

Allocator-aware containers always call `std::allocator_traits<A>::construct(m,
p, args)` to construct an object of type `T` at `p` using `args`, with `m ==
get_allocator()`. The default `construct` in `std::allocator` calls
`::new((void*)p) T(args)`(until C++20)`std::allocator` has no `construct` member
and `std::construct_at(p, args)` is called when constructing elements(since
C++20), but specialized allocators may choose a different definition.

### Standard library

All standard library containers except `std::array` are
AllocatorAwareContainers:

- `std::basic_string`
- `std::deque`
- `std::forward_list`
- `std::list`
- `std::vector`
- `std::map`
- `std::multimap`
- `std::set`
- `std::multiset`
- `std::unordered_map`
- `std::unordered_multimap`
- `std::unordered_set`
- `std::unordered_multiset`

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2839 | C++11 | self move assignment of standard containers was not allowed
      | allowed but the result is unspecified

---
*Source: https://en.cppreference.com/w/cpp/named_req/AllocatorAwareContainer*
