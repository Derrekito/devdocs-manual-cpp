# std::atomic<T>::is_lock_free

```cpp
bool is_lock_free() const noexcept;  // (1) (since C++11)
bool is_lock_free() const volatile noexcept;  // (2) (since C++11)
```

Checks whether the atomic operations on all objects of this type are lock-free.

### Parameters

(none)

### Return value

`true` if the atomic operations on the objects of this type are lock-free,
`false` otherwise.

### Notes

All atomic types except for `std::atomic_flag` may be implemented using mutexes
or other locking operations, rather than using the lock-free atomic CPU
instructions. Atomic types are also allowed to be *sometimes* lock-free, e.g. if
only aligned memory accesses are naturally atomic on a given architecture,
misaligned objects of the same type have to use locks.

The C++ standard recommends (but does not require) that lock-free atomic
operations are also address-free, that is, suitable for communication between
processes using shared memory.

### Example

```cpp
#include <atomic>
#include <iostream>
#include <utility>

struct A { int a[100]; };
struct B { int x, y; };

int main()
{
    std::cout << std::boolalpha
              << "std::atomic<A> is lock free? "
              << std::atomic<A>{}.is_lock_free() << '\n'
              << "std::atomic<B> is lock free? "
              << std::atomic<B>{}.is_lock_free() << '\n';
}
```

Possible output:

```text
std::atomic<A> is lock free? false
std::atomic<B> is lock free? true
```

### See also

- **atomic_is_lock_free (C++11)** — checks if the atomic type's operations are
  lock-free (function template)
- **atomic_is_lock_free(std::shared_ptr)** — specializes atomic operations for
  `std::shared_ptr` (function template)
- **is_always_lock_free [static] (C++17)** — indicates that the type is always
  lock-free (public static member constant)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/is_lock_free*
