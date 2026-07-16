# std::uses_allocator

```cpp
template< class T, class Alloc > struct uses_allocator;  // (since C++11)
```

If `T` has a member typedef `allocator_type` which is convertible from `Alloc`
or is an alias of `std::experimental::erased_type`(library fundamentals TS), the
member constant `value` is `true`. Otherwise `value` is `false`.

### Helper variable template

```cpp
template< class T, class Alloc >
inline constexpr bool uses_allocator_v = uses_allocator<T, Alloc>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `T` uses allocator `Alloc`, `false` otherwise
  (public static member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Uses-allocator construction

There are three conventions of passing an allocator `alloc` to a constructor of
some type `T`:

- if `T` does not use a compatible allocator (`std::uses_allocator_v<T, Alloc>`
  is false), then `alloc` is ignored.
- otherwise, `std::uses_allocator_v<T, Alloc>` is true, and
- As a special case, `std::pair` is treated as a uses-allocator type even though
  `std::uses_allocator` is false for pairs (unlike e.g. `std::tuple`): see
  pair-specific overloads of `std::pmr::polymorphic_allocator::construct` and
  `std::scoped_allocator_adaptor::construct`(until
  C++20)`std::uses_allocator_construction_args`(since C++20).

The utility functions `std::make_obj_using_allocator`, and
`std::uninitialized_construct_using_allocator` may be used to explicitly create
an object following the above protocol, and
`std::uses_allocator_construction_args` can be used to prepare the argument list
that matches the flavor of uses-allocator construction expected by the type.
*(since C++20)*

### Specializations

Custom specializations of the type trait `std::uses_allocator` are allowed for
types that do not have the member typedef `allocator_type` but satisfy one of
the following two requirements:

1. `T` has a constructor which takes `std::allocator_arg_t` as the first
   argument, and `Alloc` as the second argument.
1. `T` has a constructor which takes `Alloc` as the last argument.

In the above, `Alloc` is a type that satisfies Allocator or is a pointer type
convertible to `std::experimental::pmr::memory_resource*`(library fundamentals
TS).

The following specializations are already provided by the standard library:

- **std::uses_allocator<std::tuple> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::priority_queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::stack> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_map> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_set> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_multimap> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::flat_multiset> (C++23)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::function> (C++11) (until C++17)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::promise> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::packaged_task> (C++11) (until C++17)** —
  specializes the `std::uses_allocator` type trait (class template
  specialization)

### Notes

This type trait is used by `std::tuple`, `std::scoped_allocator_adaptor`, and
`std::pmr::polymorphic_allocator`. It may also be used by custom allocators or
wrapper types to determine whether the object or member being constructed is
itself capable of using an allocator (e.g. is a container), in which case an
allocator should be passed to its constructor.

### See also

- **allocator_arg (C++11)** — an object of type `std::allocator_arg_t` used to
  select allocator-aware constructors (constant)
- **allocator_arg_t (C++11)** — tag type used to select allocator-aware
  constructor overloads (class)
- **uses_allocator_construction_args (C++20)** — prepares the argument list
  matching the flavor of uses-allocator construction required by the given type
  (function template)
- **make_obj_using_allocator (C++20)** — creates an object of the given type by
  means of uses-allocator construction (function template)
- **uninitialized_construct_using_allocator (C++20)** — creates an object of the
  given type at specified memory location by means of uses-allocator
  construction (function template)
- **scoped_allocator_adaptor (C++11)** — implements multi-level allocator for
  multi-level containers (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/uses_allocator*
