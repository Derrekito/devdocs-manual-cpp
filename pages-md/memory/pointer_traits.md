# std::pointer_traits

```cpp
template< class Ptr >
struct pointer_traits;  // (1) (since C++11)
template< class T >
struct pointer_traits<T*>;  // (2) (since C++11)
```

The `pointer_traits` class template provides the standardized way to access
certain properties of pointer-like types (fancy pointers, such as
`boost::interprocess::offset_ptr`). The standard template
`std::allocator_traits` relies on `pointer_traits` to determine the defaults for
various typedefs required by Allocator.

1) The non-specialized `pointer_traits` conditionally declares the following
   members: Let `/*element-type-of*/<Ptr>` be `Ptr::element_type` if present;
   otherwise, `T` if `Ptr` is a class template specialization `Template<T,
   Args...>`, where `Args...` is zero or more type arguments; otherwise, not
   defined. If `/*element-type-of*/<Ptr>` is not defined, the primary template
   has no members specified in this page. Member types Type Definition `pointer`
   `Ptr` `element_type` `/*element-type-of*/<Ptr>` `difference_type`
   `Ptr::difference_type` if present, otherwise `std::ptrdiff_t` Member alias
   templates Template Definition `template< class U > using rebind`
   `Ptr::rebind<U>` if exists, otherwise `Template<U, Args...>` if `Ptr` is a
   template specialization `Template<T, Args...>` Member functions pointer_to
   [static] obtains a dereferenceable pointer to its argument (public static
   member function)

2) A specialization is provided for pointer types, `T*`, which declares the
   following members: Member types Type Definition `pointer` `T*` `element_type`
   `T` `difference_type` `std::ptrdiff_t` Member alias templates Template
   Definition `template< class U > using rebind` `U*` Member functions
   pointer_to [static] obtains a dereferenceable pointer to its argument (public
   static member function)

### Optional member functions of program-defined specializations

- **to_address [static] (C++20)(optional)** — obtains a raw pointer from a fancy
  pointer (inverse of `pointer_to`) (public static member function)

### Notes

The rebind member template alias makes it possible, given a pointer-like type
that points to `T`, to obtain the same pointer-like type that points to `U`. For
example,

```cpp
using another_pointer = std::pointer_traits<std::shared_ptr<int>>::rebind<double>;
static_assert(std::is_same<another_pointer, std::shared_ptr<double>>::value);
```

A specialization for user-defined fancy pointer types may provide an additional
static member function `to_address` to customize the behavior of
`std::to_address`.
*(since C++20)*

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_constexpr_memory` | 201811L | (C++20) | constexpr in
      `std::pointer_traits`

### Example

```cpp
#include <iostream>
#include <memory>

template<class Ptr>
struct BlockList
{
    // Predefine a memory block
    struct block;

    // Define a pointer to a memory block from the kind of pointer Ptr s
    // If Ptr is any kind of T*, block_ptr_t is block*
    // If Ptr is smart_ptr<T>, block_ptr_t is smart_ptr<block>
    using block_ptr_t = typename std::pointer_traits<Ptr>::template rebind<block>;

    struct block
    {
        std::size_t size{};
        block_ptr_t next_block{};
    };

    block_ptr_t free_blocks;
};

int main()
{
    [[maybe_unused]]
    BlockList<int*> bl1;
    // The type of bl1.free_blocks is BlockList<int*>:: block*

    BlockList<std::shared_ptr<char>> bl2;
    // The type of bl2.free_blocks is
    // std::shared_ptr<BlockList<std::shared_ptr<char>>::block>
    std::cout << bl2.free_blocks.use_count() << '\n';
}
```

Output:

```text
​0​
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3545 | C++11 | primary template caused hard error when `element_type` is
      invalid | made SFINAE-friendly

### See also

- **allocator_traits (C++11)** — provides information about allocator types
  (class template)
- **addressof (C++11)** — obtains actual address of an object, even if the `&`
  operator is overloaded (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/pointer_traits*
