# std::make_shared, std::make_shared_for_overwrite

Constructs an object and wraps it in a `std::shared_ptr` in one step.
Prefer this over `std::shared_ptr<T>(new T(args...))` for two reasons:
`std::shared_ptr<T>(new T(args...))` performs two allocations (the
object, then the control block) where `make_shared` typically performs
just one; and before C++17, an exception thrown while evaluating a
sibling argument in the same call could leak the raw `new T(...)`
before the `shared_ptr` took ownership — `make_shared` never has that
gap.

```cpp skip
std::make_shared<T>(args...);          // construct T(args...)          (since C++11)
std::make_shared<T[]>(n);              // array, value-initialized      (since C++20)
std::make_shared<T[N]>();              // fixed-size array, value-init  (since C++20)
std::make_shared<T[]>(n, val);         // array, every element = val    (since C++20)
std::make_shared_for_overwrite<T>();   // default-init, skips value-init (since C++20)
std::make_shared_for_overwrite<T[]>(n); // array, default-init only    (since C++20)
```

### What you provide

- **T** — the type to construct; an array type (`T[]` or `T[N]`) to
  get an array `shared_ptr` (C++20).
- **args** — forwarded to `T`'s constructor (non-array form only).
- **N / n** — element count for the array forms (`N` fixed at compile
  time, `n` at runtime).
- **u** — an initial value copied into every array element, for the
  overloads that take one.

### Guarantees and costs

- Returns `std::shared_ptr<T>`. The control block and the object are
  usually allocated together as a single heap allocation — the
  standard recommends, but does not require, this; every known
  implementation does it.
- May throw `std::bad_alloc`, or whatever `T`'s constructor throws; on
  either, the function has no effect. If an exception is thrown while
  constructing an array, already-constructed elements are destroyed in
  reverse order (since C++20).
- Elements of an array overload are constructed in ascending address
  order and destroyed in the reverse of that order when their lifetime
  ends.
- `make_shared_for_overwrite` skips value-initialization — cheaper,
  but the object (or every element) starts with indeterminate value.
- Unlike `std::shared_ptr`'s constructors, `make_shared` does not
  accept a custom deleter, and it requires public access to the
  constructor it calls (a `shared_ptr` constructor built from a raw
  `new T(...)` can call a non-public constructor if invoked somewhere
  with access to it).

### Gotchas

- If any `std::weak_ptr` still references the shared control block
  after every `shared_ptr` owner is gone, the memory for `T` is not
  released until that `weak_ptr` is destroyed too — because object and
  control block share one allocation, a single lingering `weak_ptr`
  pins the whole thing. This can matter when `sizeof(T)` is large.
  With the two-allocation `shared_ptr<T>(new T(...))` form, the
  object's memory is freed independently of the control block's.
- `make_shared_for_overwrite` skips initialization entirely — reading
  before writing is undefined behavior.
- `make_shared` uses `::new` directly; a class-specific `operator new`
  on `T` is bypassed, unlike with `shared_ptr<T>(new T(...))`.
- `std::shared_ptr` supports array types since C++17, but
  `make_shared`'s array overloads didn't arrive until C++20 — on
  C++17, array `shared_ptr`s must be constructed the raw-pointer way.

### Example

```cpp c++20
#include <iostream>
#include <memory>

struct C
{
    C(int i) : i(i) {}
    C(int i, float f) : i(i), f(f) {}
    int i;
    float f{};
};

int main()
{
    auto sp1 = std::make_shared<C>(1);
    auto sp2 = std::make_shared<C>(2, 3.0f);
    std::cout << "sp1: { i=" << sp1->i << ", f=" << sp1->f << " }\n"
              << "sp2: { i=" << sp2->i << ", f=" << sp2->f << " }\n";

    auto arr = std::make_shared<double[]>(3, 2.0); // every element 2.0
    std::cout << "arr: [" << arr[0] << ", " << arr[1] << ", " << arr[2] << "]\n";
}
```

```text
sp1: { i=1, f=0 }
sp2: { i=2, f=3 }
arr: [2, 2, 2]
```

### Reference

```cpp skip
template< class T, class... Args >
shared_ptr<T> make_shared( Args&&... args );  // (since C++11) (T is not array)
template< class T >
shared_ptr<T> make_shared( std::size_t N );  // (since C++20) (T is U[])
template< class T >
shared_ptr<T> make_shared();  // (since C++20) (T is U[N])
template< class T >
shared_ptr<T> make_shared( std::size_t N, const std::remove_extent_t<T>& u );  // (since C++20) (T is U[])
template< class T >
shared_ptr<T> make_shared( const std::remove_extent_t<T>& u );  // (since C++20) (T is U[N])
template< class T >
shared_ptr<T> make_shared_for_overwrite();  // (since C++20) (T is not U[])
template< class T >
shared_ptr<T> make_shared_for_overwrite( std::size_t N );  // (since C++20) (T is U[])
```

Formally: overload (1) enables `shared_from_this` on the constructed
object, same as the raw-pointer `shared_ptr` constructor would.

Feature-test macros: `__cpp_lib_shared_ptr_arrays` — `201707L`
(C++20, array support in `make_shared`); `__cpp_lib_smart_ptr_for_overwrite`
— `202002L` (C++20, the `_for_overwrite` family).

### See also

- **(constructor)** — constructs a `shared_ptr` directly, from a raw pointer
- **allocate_shared, allocate_shared_for_overwrite (C++20)** — same, with a custom allocator
- **enable_shared_from_this (C++11)** — lets an object hand out a `shared_ptr` to itself
- **make_unique, make_unique_for_overwrite (C++14)(C++20)** — the `unique_ptr` equivalent

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/make_shared*
