# std::shared_ptr

`std::shared_ptr` gives an object shared ownership: any number of
`shared_ptr`s can point at the same object, and it is destroyed once
the last one is destroyed or reset. Sharing costs a heap-allocated
*control block* (holding the ref count and the deleter) plus an atomic
increment/decrement on every copy and destruction — reach for
`unique_ptr` by default and use `shared_ptr` only when ownership is
genuinely shared.

```cpp skip
auto p = std::make_shared<T>(args...);       // preferred: one allocation
std::shared_ptr<T> p(new T(args...));         // two allocations; avoid
std::shared_ptr<T> p2 = p;                    // shares ownership, use_count++
p.reset();                                    // this owner drops its share
p.use_count();
std::shared_ptr<U> alias(p, &p->member);      // aliasing constructor
```

### Member table

- **Member types** — `element_type` (`T`, or `remove_extent_t<T>`
  since C++17); `weak_type` (`std::weak_ptr<T>`, since C++17).
- **(constructor) / (destructor) / operator=** — construct, destroy,
  reassign; copyable.
- **Modifiers** — `reset`, `swap`.
- **Observers** — `get` (the *stored* pointer); `operator*` /
  `operator->`; `operator[]` (C++17, array form); `use_count`;
  `unique` (until C++20); `operator bool`; `owner_before`;
  `owner_hash` / `owner_equal` (C++26).
- **Non-member** — `make_shared` / `make_shared_for_overwrite` (C++20);
  `allocate_shared` / `allocate_shared_for_overwrite` (C++20);
  `static_pointer_cast`, `dynamic_pointer_cast`, `const_pointer_cast`,
  `reinterpret_pointer_cast` (C++17); `get_deleter`; comparison
  operators, `<=>` (C++20, replacing the individually removed
  relational operators); `operator<<`; `std::swap` (C++11); the
  `std::atomic_*` free functions (deprecated in C++20 — use
  `std::atomic<std::shared_ptr>` instead).
- **Helper classes** — `std::atomic<std::shared_ptr>` (C++20);
  `std::hash<std::shared_ptr>` (C++11).

### Guarantees and costs

- Every member function, including copy construction and copy
  assignment, is safe to call concurrently on *different* `shared_ptr`
  instances, even copies that share ownership of the same object — no
  extra synchronization needed. Calling a non-const member function on
  the *same* instance from multiple threads without synchronization is
  a data race; use the `std::atomic_*` overloads (deprecated) or
  `std::atomic<std::shared_ptr>` (C++20) to avoid it. This guarantee
  covers the control block's bookkeeping only, not the managed
  object's own data.
- Typical layout: a `shared_ptr` holds two pointers — the stored
  pointer (returned by `get()`) and a pointer to the control block. The
  control block holds the managed pointer or object, the type-erased
  deleter and allocator, the strong (shared) count, and the weak
  count.
- `make_shared`/`allocate_shared` perform one allocation for the
  control block and the object together; constructing from a raw
  pointer allocates the control block separately from the object.
- The strong count is typically incremented with a relaxed atomic
  add; decrementing needs a stronger memory order to safely destroy
  the control block once the count hits zero — so even single-threaded
  copy/destroy churn has real atomic-operation cost.
- The control block outlives the managed object if any `weak_ptr`
  still refers to it: the object is destroyed when the strong count
  reaches zero, but the control block itself isn't freed until the
  weak count also reaches zero.
- All specializations satisfy CopyConstructible, CopyAssignable, and
  LessThanComparable, and convert contextually to `bool`.

### Gotchas

- Constructing a new `shared_ptr` from another `shared_ptr`'s raw
  pointer (rather than copying the `shared_ptr` itself) builds a
  second, unrelated control block — both will eventually try to
  delete the same object. Only copy/assign a `shared_ptr` to share
  ownership.
- The aliasing constructor lets `get()` return a pointer that has
  nothing to do with what actually gets deleted (the *stored* pointer
  vs. the *managed* pointer can differ) — don't assume the two match.
- `shared_ptr` tolerates an incomplete `T` in general, but the
  raw-pointer constructor and `reset(Y*)` require a complete type at
  the call site.
- The atomic ref-count traffic is not free even without threads —
  don't default to `shared_ptr` when `unique_ptr` would do.

### Example

```cpp
#include <iostream>
#include <memory>

struct Widget { int value = 42; };

int main()
{
    auto owner = std::make_shared<Widget>();

    // aliasing constructor: shares ownership of the Widget, but the
    // stored pointer points at its `value` member instead
    std::shared_ptr<int> value_ptr(owner, &owner->value);

    std::cout << "use_count = " << owner.use_count() << '\n';
    std::cout << "*value_ptr = " << *value_ptr << '\n';
}
```

```text
use_count = 2
*value_ptr = 42
```

### Reference

Full declaration:

```cpp skip
template< class T > class shared_ptr;  // (since C++11)
```

Ownership can only be shared by copy-constructing or copy-assigning
one `shared_ptr`'s value to another; building a new `shared_ptr` from
the raw pointer owned by an existing one is undefined behavior.

`T` may be a function type, in which case the `shared_ptr` manages a
pointer to function rather than an object — a technique for keeping a
plugin or dynamic library loaded as long as any of its functions are
still referenced.

### See also

- **unique_ptr (C++11)** — unique, non-shared ownership
- **weak_ptr (C++11)** — non-owning observer of a `shared_ptr`

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr*
