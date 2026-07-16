# std::atomic

`std::atomic<T>` (C++11) gives a single trivially-copyable value
well-defined, race-free access from multiple threads without a mutex:
one thread writing while another reads is defined behavior, and the
individual operation (`load`, `store`, `++`, `fetch_add`, a
compare-exchange, ...) is indivisible. It is **not** a substitute for a
mutex around anything more than that — a *sequence* of atomic
operations (check the value, then act on it; update two atomics
together) is not itself atomic, and races between the steps are still
possible. It also isn't guaranteed to be lock-free: check
`is_lock_free()` before relying on that property.

```cpp skip
std::atomic<int> a{0};
a.store(5);  a.load();               // write / read
++a;  a += 3;                        // atomic increment / add
a.fetch_add(3);                      // same as +=, returns the old value
int expected = 5;
a.compare_exchange_strong(expected, 10);  // CAS: if a==expected, set to 10
a.is_lock_free();                    // does *this* object avoid locking?
std::atomic_int ai;                  // alias for atomic<int>
```

### What you provide

- **T** — for the primary template, any TriviallyCopyable type that is
  also CopyConstructible, CopyAssignable, MoveConstructible, and
  MoveAssignable; anything else is ill-formed. Partial specializations
  exist for pointers (`atomic<U*>`, with `fetch_add`/`fetch_sub`) and,
  since C++20, `std::shared_ptr<U>` and `std::weak_ptr<U>`.
  Instantiating with an integral or floating-point type additionally
  unlocks `fetch_add`, `fetch_sub`, and (integral only) `fetch_and`/
  `fetch_or`/`fetch_xor`.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | constructs the atomic object |
| `operator=` | atomically stores a value |
| `is_lock_free()` | true if operations on *this* object don't lock |
| `is_always_lock_free` [static] (C++17) | true if always lock-free |
| `store(v)` / `load()` | atomic write / read |
| `operator T` | same as `load()` |
| `exchange(v)` | atomically stores `v`, returns the previous value |
| `compare_exchange_weak/strong(exp, v)` | CAS: store `v` if equal to `exp` |
| `wait(old)` (C++20) | blocks until notified and value differs from `old` |
| `notify_one`/`notify_all` (C++20) | wake threads blocked in `wait` |
| `fetch_add`/`fetch_sub` | integral/pointer/float (C++20): add/subtract |
| `fetch_and`/`fetch_or`/`fetch_xor` | integral only: bitwise op |
| `operator++`/`--`, `+=`/`-=`/`&=`/`\|=`/`^=` | shorthand for `fetch_*` |

### Guarantees and costs

- `atomic` is neither copyable nor movable; there is exactly one
  object, accessed through references.
- Accesses to an atomic object may establish inter-thread
  synchronization, ordering surrounding non-atomic accesses, as
  specified by `std::memory_order` (the default is the strongest,
  sequentially consistent, ordering).
- Lock-freedom is not guaranteed. All atomic types except
  `atomic_flag` may be implemented with an internal mutex instead of
  lock-free instructions, and a type can even be lock-free only
  sometimes (e.g. misaligned objects falling back to a lock while
  aligned ones don't) — `is_always_lock_free` is a compile-time
  guarantee, `is_lock_free()` a runtime check for one object.
- On gcc and clang, some operations on wider or unusual atomic types
  require linking against `-latomic`.

### Gotchas

- Atomicity applies to one operation, not a sequence of them: `if
  (a.load() == x) a.store(y);` still has a race between the load and
  the store. Use a single `compare_exchange_strong`/`weak` for that
  pattern, or guard the whole sequence with a `mutex` — `atomic` alone
  cannot express "check, then act" safely.
- A type that isn't TriviallyCopyable (owns a pointer, has a
  user-defined copy constructor, etc.) cannot be used as `T` at all —
  the instantiation is ill-formed, not merely slow.
- Don't assume lock-freedom: an `atomic<T>` that turns out not to be
  lock-free (check `is_lock_free()`) behaves correctly but loses the
  performance reason for choosing it over a mutex-guarded value.

### Example

```cpp
#include <atomic>
#include <iostream>
#include <thread>
#include <vector>

int main()
{
    std::atomic<int> counter{0};

    std::vector<std::thread> pool;
    for (int i = 0; i < 4; ++i)
        pool.emplace_back([&]{
            for (int n = 0; n < 1000; ++n)
                ++counter;
        });

    for (auto& t : pool)
        t.join();

    std::cout << counter << '\n';
}
```

```text
4000
```

### Reference

```cpp skip
template< class T >
struct atomic;  // (1) (since C++11)
template< class U >
struct atomic<U*>;  // (2) (since C++11)
template< class U >
struct atomic<std::shared_ptr<U>>;  // (3) (since C++20)
template< class U >
struct atomic<std::weak_ptr<U>>;  // (4) (since C++20)
#define _Atomic(T) /* see below */  // (5) (since C++23)
```

Formally: the primary template requires `std::is_trivially_copyable<T>`,
`is_copy_constructible<T>`, `is_move_constructible<T>`,
`is_copy_assignable<T>`, and `is_move_assignable<T>` all `true`, or the
program is ill-formed. `atomic<bool>` uses the primary template and is
guaranteed a standard-layout struct. Type aliases are provided for
`bool` and every fixed-width/standard integer type (e.g. `atomic_int`,
`atomic_size_t`, `atomic_uint32_t`) as shorthand for
`atomic<Integral>`. `_Atomic(T)` (C++23) is the compatibility spelling
used by `<stdatomic.h>`, identical to `atomic<T>` where both are
well-formed.

### See also

- **atomic_flag** — the only atomic type guaranteed always lock-free
- **memory_order** — controls the ordering guarantees of atomic operations
- **mutex** — guards compound operations that a single atomic can't express
- **atomic<std::shared_ptr>** (C++20) — atomic shared pointer specialization
- **atomic<std::weak_ptr>** (C++20) — atomic weak pointer specialization

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic*
