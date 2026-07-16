# Standard library header <memory>

`<memory>` covers two mostly-independent concerns: smart pointers
(`unique_ptr`, `shared_ptr`, `weak_ptr`) for automatic, exception-safe
object lifetime management, and the allocator machinery (`allocator`,
`allocator_traits`, uses-allocator construction) that containers use to
obtain and release raw storage. Most everyday code only needs the
smart-pointer half; the allocator half matters when writing a custom
container, a `pmr`-aware type, or code that constructs objects directly
into memory it doesn't yet own as a live object.

Include it whenever you need an owning pointer, or one of the
`uninitialized_*` / `construct_at` / `destroy_at` algorithms used to
build objects on top of raw, uninitialized storage — the same
primitives containers use internally.

### Includes

- **`<compare>`** (C++20) — three-way comparison operator support

### Pointer traits

- **pointer_traits** (C++11) — information about pointer-like types

### Allocators

- **allocator** — the default allocator
- **allocator_traits** (C++11) — information about allocator types
- **allocation_result** (C++23) — address and actual size returned by
  `allocate_at_least`
- **allocator_arg_t** / **allocator_arg** (C++11) — tag type, and tag
  object, that selects an allocator-aware constructor overload
- **uses_allocator** (C++11) — does a type support uses-allocator
  construction?
- **uses_allocator_construction_args** (C++20) — builds the argument
  list for uses-allocator construction
- **make_obj_using_allocator** (C++20) — creates an object via
  uses-allocator construction
- **uninitialized_construct_using_allocator** (C++20) — same, at a given
  memory location

### Smart pointers

- **unique_ptr** (C++11) — unique-ownership smart pointer
- **shared_ptr** (C++11) — shared-ownership smart pointer
- **weak_ptr** (C++11) — non-owning weak reference to a
  `shared_ptr`-managed object
- **default_delete** (C++11) — `unique_ptr`'s default deleter
- **bad_weak_ptr** (C++11) — thrown when locking a `weak_ptr` whose
  object is already destroyed
- **enable_shared_from_this** (C++11) — lets an object hand out a
  `shared_ptr` to itself
- **owner_less** (C++11) — owner-based (not value-based) ordering of
  `shared_ptr`/`weak_ptr`
- **owner_hash** / **owner_equal** (C++26) — owner-based hashing and
  equality for the same
- **auto_ptr** (deprecated C++11, removed C++17) — the pre-C++11 owning
  pointer; superseded by `unique_ptr`
- **std::hash<unique_ptr>** / **std::hash<shared_ptr>** (C++11) — hash
  support
- **std::atomic<shared_ptr>** / **std::atomic<weak_ptr>** (C++20) —
  atomic specializations, see `<atomic>`

### Smart pointer creation and casts

- **make_unique** (C++14) / **make_unique_for_overwrite** (C++20) —
  creates a `unique_ptr` to a new object
- **make_shared** (C++11) / **make_shared_for_overwrite** (C++20) —
  creates a `shared_ptr` to a new object
- **allocate_shared** (C++11) / **allocate_shared_for_overwrite**
  (C++20) — same, using a given allocator
- **static_pointer_cast** / **dynamic_pointer_cast** /
  **const_pointer_cast** — casts a `shared_ptr`
- **reinterpret_pointer_cast** (C++17) — `reinterpret_cast` on a
  `shared_ptr`
- **get_deleter** — retrieves a `unique_ptr`/`shared_ptr`'s deleter, by
  type
- **operator==** and the relational operators — compares two
  `unique_ptr`s, two `shared_ptr`s, or either against `nullptr`
  (`operator<=>` added in C++20; the other relational overloads were
  removed then, synthesized from it)
- **operator<<** — writes the stored pointer to an output stream
  (`unique_ptr` support added C++20)
- **std::swap** (C++11) — specializes `std::swap` for `unique_ptr`,
  `shared_ptr`, and `weak_ptr`

### Smart pointer adaptors

- **out_ptr_t** / **inout_ptr_t** (C++23) — adapts a smart pointer to a
  foreign (typically C) API that fills in a raw pointer through an
  out-parameter
- **out_ptr** / **inout_ptr** (C++23) — creates an `out_ptr_t` /
  `inout_ptr_t`

### Uninitialized storage

- **uninitialized_copy** / **uninitialized_copy_n** (C++11 for `_n`) —
  copies a range, or N elements, into uninitialized memory
- **uninitialized_fill** / **uninitialized_fill_n** — assigns a value
  across a range, or N elements, of uninitialized memory
- **uninitialized_move** / **uninitialized_move_n** (C++17) — moves a
  range, or N elements, into uninitialized memory
- **uninitialized_default_construct** /
  **uninitialized_default_construct_n** (C++17) — default-initializes
  objects over uninitialized memory
- **uninitialized_value_construct** / **uninitialized_value_construct_n**
  (C++17) — value-initializes objects over uninitialized memory
- **construct_at** (C++20) — constructs an object at a given address
- **destroy_at** / **destroy** / **destroy_n** (C++17) — destroys one
  object, a range of objects, or N objects
- **get_temporary_buffer** / **return_temporary_buffer** (deprecated
  C++17, removed C++20) — obtains and frees uninitialized storage

Since C++20, `std::ranges::` niebloid versions exist for the whole
family above (`uninitialized_copy`, `uninitialized_copy_n`,
`uninitialized_fill`, `uninitialized_fill_n`, `uninitialized_move`,
`uninitialized_move_n`, `uninitialized_default_construct`,
`uninitialized_default_construct_n`, `uninitialized_value_construct`,
`uninitialized_value_construct_n`, `construct_at`, `destroy_at`,
`destroy`, and `destroy_n`), each taking a range or an
iterator+sentinel pair instead of an iterator pair.

### Miscellaneous

- **addressof** (C++11) — an object's real address, even with
  `operator&` overloaded
- **to_address** (C++20) — obtains a raw pointer from a pointer-like
  type, without dereferencing it
- **align** (C++11) — aligns a pointer within a buffer
- **assume_aligned** (C++20) — tells the compiler a pointer is aligned
  to N
- **start_lifetime_as** / **start_lifetime_as_array** (C++23) —
  implicitly starts an object's lifetime over existing storage, reusing
  its object representation
- **hash** / **atomic** (C++11) — forward-declared here for the
  specializations above; defined in `<functional>` and `<atomic>`

### Garbage collector support (removed in C++23)

- **pointer_safety** (C++11) — the pointer safety models
- **declare_reachable** / **undeclare_reachable** (C++11) — marks or
  unmarks an object as reachable
- **declare_no_pointers** / **undeclare_no_pointers** (C++11) — marks or
  unmarks a memory region as pointer-free
- **get_pointer_safety** (C++11) — returns the current pointer safety
  model

### Deprecated

- **raw_storage_iterator** (deprecated C++17, removed C++20) — output
  iterator that stores algorithm results into uninitialized memory
- **atomic_load** / **atomic_store** / **atomic_exchange** /
  **atomic_compare_exchange_weak** / **atomic_compare_exchange_strong**
  and their `_explicit` forms (deprecated in C++20) — free-function
  atomic operations on `shared_ptr`; use `std::atomic<shared_ptr<T>>`
  instead

---
*Source: https://en.cppreference.com/w/cpp/header/memory*
