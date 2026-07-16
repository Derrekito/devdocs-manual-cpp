# std::weak_ptr

`std::weak_ptr` observes an object owned by a `shared_ptr` without
owning it — holding one does not keep the object alive. It exists for
two jobs: safely checking whether a shared object is still around
(and briefly promoting to a `shared_ptr` to use it), and breaking
reference cycles between `shared_ptr`s that would otherwise leak
forever.

```cpp skip
std::weak_ptr<T> w = shared;      // observe; does not bump the strong count
if (auto sp = w.lock())           // sp is non-null only if still alive
    use(*sp);
w.expired();                      // snapshot only — see Gotchas
w.use_count();                    // number of owning shared_ptrs, for debugging
```

### Member table

- **Member types** — `element_type` (`T`, or `remove_extent_t<T>`
  since C++17).
- **(constructor) / (destructor) / operator=** — construct, destroy,
  reassign.
- **Modifiers** — `reset`, `swap`.
- **Observers** — `use_count`; `expired`; `lock` (promote to
  `shared_ptr`); `owner_before`; `owner_hash` / `owner_equal` (C++26).
- **Non-member** — `std::swap` (C++11).
- **Helper classes** — `std::atomic<std::weak_ptr>` (C++20).

### Guarantees and costs

- `lock()` is the atomic, race-free way to use a possibly-expired
  object: it returns a `shared_ptr` that owns the object if it's still
  alive, or an empty `shared_ptr` otherwise, in one step.
- Typical layout mirrors `shared_ptr`: a `weak_ptr` stores a pointer to
  the control block and the stored pointer of the `shared_ptr` it was
  built from. Keeping a separate stored pointer is what makes
  `shared_ptr` → `weak_ptr` → `shared_ptr` round-trip correctly, even
  for a `shared_ptr` created via the aliasing constructor.
- A `weak_ptr` keeps the *control block* alive, not the managed
  object — the object is destroyed as soon as the last `shared_ptr`
  owner is gone, regardless of how many `weak_ptr`s still point at the
  control block.
- Since C++26, `weak_ptr` can be used as a key in unordered
  associative containers (`__cpp_lib_smart_ptr_owner_equality`).

### Gotchas

- `weak_ptr` has no `operator*`/`operator->` — you cannot dereference
  it directly; call `lock()` first and check the result.
- `expired()` is only a snapshot: in threaded code, the last owning
  `shared_ptr` can be dropped right after the check returns. `lock()`
  is the safe alternative because the check and the acquire happen
  atomically together.
- A cycle of `shared_ptr`s that reference each other never lets any of
  their strong counts reach zero, leaking the whole cycle; the fix is
  to make one direction of the cycle a `weak_ptr` instead.

### Example

```cpp
#include <iostream>
#include <memory>

int main()
{
    auto sp = std::make_shared<int>(42);
    std::weak_ptr<int> wp = sp;

    std::cout << "use_count (shared only) = " << sp.use_count() << '\n';

    sp.reset();   // last shared_ptr owner gone

    if (auto locked = wp.lock())
        std::cout << "still alive: " << *locked << '\n';
    else
        std::cout << "expired\n";
}
```

```text
use_count (shared only) = 1
expired
```

### Reference

Full declaration:

```cpp skip
template< class T > class weak_ptr;  // (since C++11)
```

`weak_ptr` models *temporary* ownership: an object needing conditional
access converts a `weak_ptr` to `shared_ptr` to extend the object's
lifetime for as long as that temporary `shared_ptr` is held, even if
the original `shared_ptr` is destroyed meanwhile.

Defect report LWG 3001 (applied to C++17): `element_type` had not been
updated for array support; corrected.

### See also

- **shared_ptr (C++11)** — the owning counterpart `weak_ptr` observes
- **unique_ptr (C++11)** — unique, non-shared ownership

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr*
