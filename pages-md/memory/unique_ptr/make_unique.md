# std::make_unique, std::make_unique_for_overwrite

Constructs an object and wraps it in a `std::unique_ptr` in one step.
Prefer this over `std::unique_ptr<T>(new T(args...))`: with the raw
form, if the surrounding expression throws after the `new` but before
the `unique_ptr` takes ownership (e.g. evaluating another argument in
the same call), the object leaks; `make_unique` closes that gap.

```cpp skip
std::make_unique<T>(args...);          // construct T(args...)          (since C++14)
std::make_unique<T>();                 // default-initialize T          (since C++14)
std::make_unique<T[]>(size);           // array, value-initialized      (since C++14)
std::make_unique_for_overwrite<T>();   // default-init, skips value-init (since C++20)
std::make_unique_for_overwrite<T[]>(size);  // array, default-init only (since C++20)
```

`constexpr` since C++23. `make_unique<T[N]>` (array of *known* bound)
is explicitly deleted — only runtime-sized arrays are supported.

### What you provide

- **T** — the type to construct; an unbounded array type (`T[]`) to
  get an array `unique_ptr`.
- **args** — forwarded to `T`'s constructor (non-array form only).
- **size** — element count, for the array forms.

### Guarantees and costs

- Returns `std::unique_ptr<T>` (or `unique_ptr<T[]>` for the array
  forms).
- May throw `std::bad_alloc`, or whatever `T`'s constructor throws; on
  either, the function has no effect (no leak).
- The array overload value-initializes every element
  (`make_unique<T[]>`); `make_unique_for_overwrite` skips that
  initialization — cheaper, but every element (or the single object)
  starts with indeterminate value, so you must write to it before
  reading.
- Unlike `std::make_shared`, there is no allocator-aware counterpart
  (no `allocate_unique`): a custom deleter type would be needed to
  carry the allocator, which the standard hasn't added.

### Gotchas

- `make_unique_for_overwrite` skips initialization entirely — reading
  an element before writing it is undefined behavior, same as reading
  an uninitialized local.
- `make_unique<T[N]>()` with a compile-time bound doesn't compile by
  design; use `make_unique<T[]>(N)` for a runtime-sized array instead.

### Example

```cpp c++20
#include <cstddef>
#include <iostream>
#include <memory>
#include <numeric>

struct Vec3
{
    int x, y, z;
    friend std::ostream& operator<<(std::ostream& os, const Vec3& v)
    {
        return os << "{ x=" << v.x << ", y=" << v.y << ", z=" << v.z << " }";
    }
};

int main()
{
    auto v1 = std::make_unique<Vec3>();          // default-initialized
    auto v2 = std::make_unique<Vec3>(0, 1, 2);    // constructor args

    auto arr = std::make_unique_for_overwrite<int[]>(5);
    std::iota(arr.get(), arr.get() + 5, 1);       // must write before reading

    std::cout << "v1: " << *v1 << '\n'
              << "v2: " << *v2 << '\n'
              << "arr[4]: " << arr[4] << '\n';
}
```

```text
v1: { x=0, y=0, z=0 }
v2: { x=0, y=1, z=2 }
arr[4]: 5
```

### Reference

```cpp skip
template< class T, class... Args >
unique_ptr<T> make_unique( Args&&... args );  // (since C++14) (non-array T)
template< class T >
unique_ptr<T> make_unique( std::size_t size );  // (since C++14) (T is U[])
template< class T, class... Args >
/* unspecified */ make_unique( Args&&... args ) = delete;  // (T is U[N])
template< class T >
unique_ptr<T> make_unique_for_overwrite();  // (since C++20) (non-array T)
template< class T >
unique_ptr<T> make_unique_for_overwrite( std::size_t size );  // (since C++20) (T is U[])
```

`constexpr` on the non-deleted overloads since C++23.

Feature-test macros: `__cpp_lib_make_unique` — `201304L` (C++14);
`__cpp_lib_smart_ptr_for_overwrite` — `202002L` (C++20,
`make_unique_for_overwrite`); `__cpp_lib_constexpr_memory` —
`202202L` (C++23, `constexpr` support).

### See also

- **(constructor)** — constructs a `unique_ptr` directly, from a raw pointer
- **make_shared, make_shared_for_overwrite (C++20)** — the `shared_ptr` equivalent

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/make_unique*
