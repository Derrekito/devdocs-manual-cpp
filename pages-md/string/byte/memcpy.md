# std::memcpy

Copies `count` bytes from `src` to `dest`, treating both as raw
`unsigned char` arrays — the fastest memory-to-memory copy the library
offers. It's only well-defined for objects that are TriviallyCopyable;
copying anything else is unspecified or undefined behavior even though
it compiles. The source and destination must not overlap — use
`std::memmove` when they might.

```cpp skip
std::memcpy(dest, src, count);   // count bytes; dest and src must not overlap
```

### What you provide

- **dest** — pointer to the destination memory; must be valid (non-null).
- **src** — pointer to the source memory; must be valid (non-null).
- **count** — number of bytes to copy (`std::size_t`).

### Guarantees and costs

- Returns `dest`.
- Usually the fastest raw copy available: unlike `std::strcpy` it doesn't
  scan the data, and unlike `std::memmove` it doesn't guard against
  overlap.
- Well-defined only when `dest` and `src` don't overlap and the copied
  objects are TriviallyCopyable; otherwise the behavior is unspecified,
  and may be undefined.
- May be used to implicitly create objects in the destination buffer.
- Many compilers recognize hand-written byte-copy loops and replace them
  with a call to `memcpy`.

### Gotchas

- Overlapping `src`/`dest` is undefined behavior — reach for
  `std::memmove` when the ranges might overlap.
- Copying a type that isn't TriviallyCopyable (a user-defined copy
  constructor or destructor, virtual functions, etc.) is undefined
  behavior, even though the call compiles fine.
- `dest` or `src` being null is undefined behavior even when `count` is
  zero — don't rely on a zero-length call being a safe no-op.
- It's the standard-sanctioned way to reinterpret an object's bytes as
  another type when strict aliasing forbids doing so directly (e.g.
  reading a `double`'s bit pattern into an `std::int64_t`).

### Example

```cpp
#include <cstdint>
#include <cstring>
#include <iostream>
#include <string>

int main()
{
    // simple byte copy
    char source[] = "once upon a daydream...";
    char dest[4];
    std::memcpy(dest, source, sizeof dest);
    std::cout << "dest = " << std::string(dest, 4) << '\n';

    // reinterpreting bits without violating strict aliasing
    double d = 0.1;
    std::int64_t n;
    std::memcpy(&n, &d, sizeof d);
    std::cout << std::hex << n << std::dec << '\n';
}
```

```text
dest = once
3fb999999999999a
```

### Reference

Full declaration:

```cpp skip
void* memcpy( void* dest, const void* src, std::size_t count );
```

### See also

- **memmove** — same, but safe when the buffers overlap
- **memset** — fills a buffer with a byte value
- **is_trivially_copyable** (C++11) — checks whether a type may safely be
  copied with `memcpy`
- **copy** — copies a range of elements (algorithm, not raw bytes)

**C documentation for `memcpy`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/memcpy*
