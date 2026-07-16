# std::function

`std::function<R(Args...)>` (C++11) is a general-purpose, type-erased
wrapper that can store and invoke any callable matching a given
signature — a free function, a lambda (with or without captures), a
`std::bind` expression, a function object, or a pointer to a member
function or data member. Use it when you need to hold callables of
*different* underlying types uniformly (a callback table, a stored
handler); when the callable's type is known at each call site, a plain
template parameter or `auto` lambda is cheaper — see Gotchas.

```cpp skip
std::function<R(Args...)> f = free_function;
std::function<R(Args...)> f = [captures](Args...) { ... };
std::function<R(Args...)> f = SomeFunctor{};
std::function<R(Args...)> f = std::bind(fn, args...);
std::function<R(const Obj&, Args...)> f = &Obj::member_fn;   // via bound object
f(args...);                          // invoke; throws if empty
if (f) ...                           // false iff empty
```

### What you provide

- **R** — the return type the wrapper reports; anything convertible
  from what the stored callable actually returns.
- **Args...** — the parameter types the wrapper accepts; the stored
  callable must be invocable with them.
- The callable itself must be CopyConstructible — `std::function` is
  copyable, so its target must be too (this rules out storing a
  move-only lambda directly).

### Member functions

| Member | What it does |
| --- | --- |
| `operator()(args...)` | invokes the target; throws if empty |
| `operator bool` | true iff a target is stored |
| `operator=` | assigns a new target (or clears, from `nullptr`) |
| `swap(other)` | exchanges targets |
| `target<T>()` | pointer to the stored target if its type is `T`, else null |
| `target_type()` | `typeid` of the stored target |

Non-members: `std::swap(std::function&, std::function&)`.

### Guarantees and costs

- Type erasure typically costs a virtual call per invocation, plus a
  heap allocation for targets too large for the implementation's small-
  object buffer (most captures beyond a pointer or two overflow it).
  This makes `std::function` measurably slower than calling a lambda
  or function pointer directly.
- Calling an empty `std::function` throws `std::bad_function_call`
  rather than being undefined behavior.
- `std::function` is CopyConstructible and CopyAssignable as long as
  its current target is (or it's empty).

### Gotchas

- Guard calls with `if (f) f(...);` when the callable may be unset —
  an empty `std::function` throws on invocation, it doesn't silently
  no-op.
- In hot paths, prefer a template parameter (`template <class F> void
  run(F&& f)`) or `auto` for a lambda — `std::function` pays for
  runtime polymorphism you don't need when the type is known at
  compile time. From C++23, `std::move_only_function` is the
  alternative when you need type erasure but not copyability (and
  therefore no CopyConstructible requirement on the target).
- A captured reference or pointer inside the stored callable is not
  kept alive by `std::function` — the wrapper keeps the *callable*
  alive, not whatever it captured by reference; a dangling capture
  still dangles.
- A `std::function` whose result type `R` is a reference can bind a
  dangling temporary: a lambda without a trailing return type always
  returns by value (a prvalue), so `std::function<const int&()>`
  wrapping `[]{ return 42; }` binds the returned reference to a
  temporary that's gone once `operator()` returns. Until C++23 this
  compiled and was undefined behavior on use; since C++23 it's
  ill-formed to construct at all. Returning an actual reference (a
  `static` or a captured object) is fine either way.

### Example

```cpp
#include <functional>
#include <iostream>

struct Foo
{
    Foo(int num) : num_(num) {}
    void print_add(int i) const { std::cout << num_ + i << '\n'; }
    int num_;
};

void print_num(int i) { std::cout << i << '\n'; }

int main()
{
    // store a free function
    std::function<void(int)> f_display = print_num;
    f_display(-9);

    // store a lambda
    std::function<void()> f_display_42 = [] { print_num(42); };
    f_display_42();

    // store a call to a member function, bound to an object
    using std::placeholders::_1;
    const Foo foo(314159);
    std::function<void(int)> f_add =
        std::bind(&Foo::print_add, foo, _1);
    f_add(1);

    // store a call to a data member accessor
    std::function<int(const Foo&)> f_num = &Foo::num_;
    std::cout << "num_: " << f_num(foo) << '\n';
}
```

```text
-9
42
314160
num_: 314159
```

### Reference

```cpp skip
template< class >
class function; /* undefined */  // (since C++11)
template< class R, class... Args >
class function<R(Args...)>;  // (since C++11)
```

Formally: if a `std::function` contains no target it is called
*empty*; `operator bool` reports this. `result_type` names `R`.
`argument_type`/`first_argument_type`/`second_argument_type` (present
when `Args...` has exactly one or two types) were deprecated in C++17
and removed in C++20. The `assign` member function was removed in
C++17. `operator==`/`operator!=` against `nullptr` (to test emptiness)
were removed in C++20 in favor of `operator bool`. CTAD deduction
guides were added in C++17. The `std::uses_allocator` specialization
existed C++11 through C++17 and was then removed.

### See also

- **move_only_function (C++23)** — like `function`, but move-only
- **bad_function_call (C++11)** — thrown when invoking an empty `std::function`
- **bind_front (C++20)** — binds args to a callable, no placeholder syntax
- **mem_fn (C++11)** — turns a pointer to member into a callable object

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function*
