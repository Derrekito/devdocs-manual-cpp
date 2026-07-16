# Coroutine support (C++20)

The coroutine support library defines several types that provide compile and
run-time support for coroutines.

### Coroutine traits

- **coroutine_traits (C++20)** — trait type for discovering coroutine promise
  types (class template)

### Coroutine handle

- **coroutine_handle (C++20)** — used to refer to a suspended or executing
  coroutine (class template)

### No-op coroutines

- **noop_coroutine (C++20)** — creates a coroutine handle that has no observable
  effects when resumed or destroyed (function)
- **noop_coroutine_promise (C++20)** — used for coroutines with no observable
  effects (class)
- **noop_coroutine_handle (C++20)** —
  `std::coroutine_handle<std::noop_coroutine_promise>`, intended to refer to a
  no-op coroutine (typedef)

### Trivial awaitables

- **suspend_never (C++20)** — indicates that an await-expression should never
  suspend (class)
- **suspend_always (C++20)** — indicates that an await-expression should always
  suspend (class)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_impl_coroutine` | 201902L | (C++20) | Coroutines (compiler support)
  `__cpp_lib_coroutine` | 201902L | (C++20) | Coroutines (library support)
  `__cpp_lib_generator` | 202207L | (C++23) | `std::generator`: synchronous
      coroutine generator for ranges

### See also

- **generator (C++23)** — A `view` that represents synchronous coroutine
  generator (class template)

---
*Source: https://en.cppreference.com/w/cpp/coroutine*
