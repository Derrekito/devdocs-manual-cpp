# std::unique_ptr

`std::unique_ptr` is the default smart pointer: it owns a
heap-allocated object (or array) exclusively and destroys it when the
`unique_ptr` itself is destroyed. There is exactly one owner at a
time — it cannot be copied, only moved — so it costs nothing beyond a
raw pointer and is the right first choice whenever an object doesn't
need to be shared.

```cpp skip
auto p = std::make_unique<T>(args...);          // (since C++14)
std::unique_ptr<T> p(new T(args...));            // works, prefer make_unique
auto arr = std::make_unique<T[]>(n);             // array form (since C++14)
std::unique_ptr<T, Deleter> p(raw, deleter);     // custom deleter
p->member; *p; p.get(); p.reset(); p.release();
```

### Member table

- **Member types** — `pointer` (from `Deleter::pointer`, else `T*`);
  `element_type` (`T`); `deleter_type` (`Deleter`).
- **(constructor) / (destructor) / operator=** — construct, destroy,
  reassign; move-only.
- **Modifiers** — `release` (give up ownership, return the raw
  pointer); `reset` (destroy the current object, take a new one);
  `swap`.
- **Observers** — `get` (borrow the raw pointer); `get_deleter`;
  `operator bool`.
- **Single-object form** — `operator*`, `operator->`.
- **Array form (`unique_ptr<T[]>`)** — `operator[]`.
- **Non-member** — `make_unique` (C++14) / `make_unique_for_overwrite`
  (C++20); comparison operators, `<=>` (C++20); `operator<<` (C++20);
  `std::swap` (C++11).
- **Helper classes** — `std::hash<std::unique_ptr>` (C++11).

### Guarantees and costs

- Satisfies MoveConstructible and MoveAssignable, but neither
  CopyConstructible nor CopyAssignable — ownership transfers, it never
  duplicates.
- A `const unique_ptr` cannot transfer ownership (only a non-const one
  can), so an object owned through a `const unique_ptr` is pinned to
  the scope where the pointer was created.
- `unique_ptr<T>` may be built for an incomplete `T` (handy for the
  Pimpl idiom). With the default deleter, `T` must be complete at the
  point the deleter actually runs — the destructor, move-assignment,
  and `reset`. `shared_ptr` differs here: it can't be *constructed*
  from a raw pointer to an incomplete type but *can* be destroyed
  where `T` is incomplete.
- `unique_ptr<Derived>` converts implicitly to `unique_ptr<Base>`. The
  resulting pointer deletes through `Base`'s deleter, which is
  undefined behavior unless `~Base()` is virtual. `shared_ptr` doesn't
  have this hazard — it always deletes through the originally
  constructed type.
- `Deleter` need not be a plain function pointer: any FunctionObject,
  or an lvalue reference to one, that accepts a `pointer` argument
  works. This also lets `unique_ptr` manage handle types other than
  raw pointers (e.g. an offset pointer into shared memory), as long as
  the handle satisfies NullablePointer — `shared_ptr` cannot do this.
- `constexpr std::unique_ptr` is available since C++23
  (`__cpp_lib_constexpr_memory`).

### Gotchas

- Storing a `unique_ptr<Derived>` as a `unique_ptr<Base>` (or
  converting between them) is only safe if `Base`'s destructor is
  virtual; otherwise the derived part is never destroyed, silently.
- The default deleter needs `T` complete at destruction time — a class
  using the Pimpl idiom with an inline `= default` destructor in the
  header fails to compile because the incomplete type is still
  incomplete there.
- A `const unique_ptr<T>` cannot be moved out of; the object it owns
  effectively cannot outlive that scope.

### Example

```cpp
#include <iostream>
#include <memory>

struct Base
{
    virtual ~Base() { std::cout << "~Base\n"; }
};

struct Derived : Base
{
    ~Derived() override { std::cout << "~Derived\n"; }
};

int main()
{
    std::unique_ptr<Base> p = std::make_unique<Derived>();
    std::cout << "owns a Derived through a Base*\n";
}   // ~Base is virtual, so the full Derived object is destroyed here
```

```text
owns a Derived through a Base*
~Derived
~Base
```

### Reference

Full declarations:

```cpp skip
template<
    class T,
    class Deleter = std::default_delete<T>
> class unique_ptr;  // (1) (since C++11)
template <
    class T,
    class Deleter
> class unique_ptr<T[], Deleter>;  // (2) (since C++11)
```

There are two specializations: (1) manages a single object allocated
with `new`; (2) manages a dynamically-allocated array (`new[]`), and
adds `operator[]` in place of `operator*`/`operator->`.

Type requirement: `Deleter` must be a FunctionObject, or an lvalue
reference to a FunctionObject or to a function, callable with an
argument of type `unique_ptr<T, Deleter>::pointer`.

Note that if `T` is a class template specialization, using
`unique_ptr` as an operand (e.g. `!p`) requires `T`'s template
arguments to be complete, due to argument-dependent lookup.

### See also

- **shared_ptr (C++11)** — shared ownership; reference-counted
- **weak_ptr (C++11)** — non-owning observer of a `shared_ptr`

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr*
