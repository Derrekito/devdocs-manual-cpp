# std::atomic_...<std::shared_ptr>

```cpp
template< class T >
bool atomic_is_lock_free( const std::shared_ptr<T>* p );  // (1) (since C++11) (deprecated in C++20)
template< class T >
std::shared_ptr<T> atomic_load( const std::shared_ptr<T>* p );  // (2) (since C++11) (deprecated in C++20)
template< class T >
std::shared_ptr<T> atomic_load_explicit
    ( const std::shared_ptr<T>* p, std::memory_order mo );  // (3) (since C++11) (deprecated in C++20)
template< class T >
void atomic_store( std::shared_ptr<T>* p, std::shared_ptr<T> r );  // (4) (since C++11) (deprecated in C++20)
template< class T >
void atomic_store_explicit
    ( std::shared_ptr<T>* p, std::shared_ptr<T> r,
      std::memory_order mo );  // (5) (since C++11) (deprecated in C++20)
template< class T >
std::shared_ptr<T> atomic_exchange
    ( std::shared_ptr<T>* p, std::shared_ptr<T> r );  // (6) (since C++11) (deprecated in C++20)
template< class T >
std::shared_ptr<T> atomic_exchange_explicit
    ( std::shared_ptr<T>* p, std::shared_ptr<T> r,
      std::memory_order mo );  // (7) (since C++11) (deprecated in C++20)
template< class T >
bool atomic_compare_exchange_weak
    ( std::shared_ptr<T>* p, std::shared_ptr<T>* expected,
      std::shared_ptr<T> desired );  // (8) (since C++11) (deprecated in C++20)
template< class T >
bool atomic_compare_exchange_strong
    ( std::shared_ptr<T>* p, std::shared_ptr<T>* expected,
      std::shared_ptr<T> desired );  // (9) (since C++11) (deprecated in C++20)
template< class T >
bool atomic_compare_exchange_strong_explicit
    ( std::shared_ptr<T>* p, std::shared_ptr<T>* expected,
      std::shared_ptr<T> desired,
      std::memory_order success, std::memory_order failure );  // (10) (since C++11) (deprecated in C++20)
template< class T >
bool atomic_compare_exchange_weak_explicit
    ( std::shared_ptr<T>* p, std::shared_ptr<T>* expected,
      std::shared_ptr<T> desired,
      std::memory_order success, std::memory_order failure );  // (11) (since C++11) (deprecated in C++20)
```

If multiple threads of execution access the same `std::shared_ptr` object
without synchronization and any of those accesses uses a non-const member
function of `shared_ptr` then a data race will occur unless all such access is
performed through these functions, which are overloads of the corresponding
atomic access functions (`std::atomic_load`, `std::atomic_store`, etc.).

Note that the control block of a `shared_ptr` is thread-safe: different
`std::shared_ptr` objects can be accessed using mutable operations, such as
operator= or `reset`, simultaneously by multiple threads, even when these
instances are copies, and share the same control block internally.

1) Determines whether atomic access to the shared pointer pointed-to by `p` is
   lock-free.

2) Equivalent to `atomic_load_explicit(p, std::memory_order_seq_cst)`.

3) Returns the shared pointer pointed-to by `p`.

As with the non-specialized `std::atomic_load_explicit`, if `mo` is
   `std::memory_order_release` or `std::memory_order_acq_rel`, the behavior is
   undefined.

4) Equivalent to `atomic_store_explicit(p, r, std::memory_order_seq_cst)`.

5) Stores the shared pointer `r` in the shared pointer pointed-to by `p`
   atomically, equivalent to `p->swap(r)`.

As with the non-specialized `std::atomic_store_explicit`, if `mo` is
   `std::memory_order_release` or `std::memory_order_acq_rel`, the behavior is
   undefined.

6) Equivalent to `atomic_exchange_explicit(p, r, std::memory_order_seq_cst)`.

7) Stores the shared pointer `r` in the shared pointer pointed to by `p` and
   returns the value formerly pointed-to by `p`, atomically. Equivalent to
   `p->swap(r)` and returns a copy of `r` after the swap.

8) Equivalent to

`atomic_compare_exchange_weak_explicit (p, expected, desired,
   std::memory_order_seq_cst, std::memory_order_seq_cst)`.

9) Equivalent to

`atomic_compare_exchange_strong_explicit (p, expected, desired,
   std::memory_order_seq_cst, std::memory_order_seq_cst)`.
10,11) Compares the shared pointers pointed-to by `p` and `expected`.

- If they are equivalent (store the same pointer value, and either share
  ownership of the same object or are both empty), assigns `desired` into `*p`
  using the memory ordering constraints specified by `success` and returns
  `true`.
- If they are not equivalent, assigns `*p` into `*expected` using the memory
  ordering constraints specified by `failure` and returns `false`.

`atomic_compare_exchange_weak_explicit` may fail spuriously.

If `expected` is a null pointer, or `failure` is `std::memory_order_release` or
   `std::memory_order_acq_rel`, the behavior is undefined.

If `p` is a null pointer, the behaviors of these functions are all undefined.

### Parameters

- **p, expected** — a pointer to a `std::shared_ptr`
- **r, desired** — a `std::shared_ptr`
- **mo, success, failure** — memory ordering selectors of type
  `std::memory_order`

### Exceptions

These functions do not throw exceptions.

### Return value

1) `true` if atomic access is implemented using lock-free instructions.

2,3) A copy of the pointed-to shared pointer.

4,5) (none)

6,7) A copy of the formerly pointed-to shared pointer.

8-11) `true` if the shared pointers were equivalent and the exchange was
   performed, `false` otherwise.

### Notes

These functions are typically implemented using mutexes, stored in a global hash
table where the pointer value is used as the key.

The Concurrency TS offers atomic smart pointer classes `atomic_shared_ptr` and
`atomic_weak_ptr` as a replacement for the use of these functions.

These functions were deprecated in favor of the specializations of the
`std::atomic` template: std::atomic<std::shared_ptr> and
std::atomic<std::weak_ptr>.
*(since C++20)*

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2980 | C++11 | empty `shared_ptr`s are never equivalent | equivalent if
      they store the same pointer value

### See also

- **atomic_is_lock_free (C++11)** — checks if the atomic type's operations are
  lock-free (function template)
- **atomic_storeatomic_store_explicit (C++11)(C++11)** — atomically replaces the
  value of the atomic object with a non-atomic argument (function template)
- **atomic_loadatomic_load_explicit (C++11)(C++11)** — atomically obtains the
  value stored in an atomic object (function template)
- **atomic_exchangeatomic_exchange_explicit (C++11)(C++11)** — atomically
  replaces the value of the atomic object with non-atomic argument and returns
  the old value of the atomic (function template)
-
  **atomic_compare_exchange_weakatomic_compare_exchange_weak_explicitatomic_compare_exchange_strongatomic_compare_exchange_strong_explicit
  (C++11)(C++11)(C++11)(C++11)** — atomically compares the value of the atomic
  object with non-atomic argument and performs atomic exchange if equal or
  atomic load if not (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/atomic*
