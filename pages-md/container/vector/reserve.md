# std::vector<T,Allocator>::reserve

Ensures `capacity()` is at least `new_cap`, allocating once up front so
later `push_back`/`insert` calls up to that size don't reallocate. It
never changes `size()`, and it never *shrinks* capacity — for that, use
`shrink_to_fit()`. If it does reallocate, every iterator, pointer, and
reference is invalidated; if `new_cap <= capacity()`, it's a no-op and
nothing is invalidated.

```cpp skip
v.reserve(new_cap);
```

### What you provide

- **new_cap** — the minimum capacity to guarantee, in elements. Values
  `<= capacity()` are accepted and do nothing.

### Guarantees and costs

- At most linear in `size()` (existing elements must be moved or copied
  to the new storage when it does reallocate).
- Does not change `size()`; only ever grows `capacity()`, never shrinks
  it.
- `new_cap > capacity()`: reallocates — all iterators, pointers, and
  references, `end()` included, are invalidated.
- `new_cap <= capacity()`: does nothing — no iterators or references are
  invalidated.
- Throws `std::length_error` if `new_cap > max_size()`, or whatever the
  allocator throws (typically `std::bad_alloc`) on allocation failure;
  either way the vector is left unchanged (strong exception guarantee).
  *(Since C++11)* same move-constructor fallback caveat as `push_back`:
  if `T`'s move constructor isn't `noexcept` and `T` isn't
  CopyInsertable, a throw during the fallback leaves effects
  unspecified.

### Gotchas

- Calling `reserve()` before every `push_back()` defeats the vector's
  exponential growth strategy and can *increase* the total number of
  reallocations — reserve once, for a size you actually know, not per
  element.
- Can't be used to shrink: `reserve(n)` with `n < capacity()` does
  nothing; call `shrink_to_fit()` to release unused capacity.
- About to insert a whole range? Prefer the range `insert()` overload
  over `reserve()` plus repeated `push_back()` — it gets the growth
  behavior right on its own.

### Example

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> v;
    v.reserve(4);
    const int* const p = v.data();

    for (int i = 0; i < 4; ++i)
        v.push_back(i);

    std::cout << "size == 4: " << (v.size() == 4) << '\n';
    std::cout << "capacity >= 4: " << (v.capacity() >= 4) << '\n';
    std::cout << "no reallocation: " << (v.data() == p) << '\n';
}
```

```text
size == 4: 1
capacity >= 4: 1
no reallocation: 1
```

### Reference

```cpp skip
void reserve( size_type new_cap );  // (until C++20)
constexpr void reserve( size_type new_cap );  // (since C++20)
```

Requires `T` to be MoveInsertable *(since C++11)*. Defect reports: LWG
329 clarified that reallocation is triggered only by `capacity()`, not
by the size last passed to `reserve()`; LWG 2033 added the MoveInsertable
requirement.

### See also

- **capacity** — the number of elements the current storage can hold
- **max_size** — the largest size the container could ever reach
- **resize** — changes `size()` (and may also change capacity)
- **shrink_to_fit** — the inverse: releases unused capacity

---
*Source: https://en.cppreference.com/w/cpp/container/vector/reserve*
