# std::move_only_function::operator()

```cpp
R operator()( Args... args ) /*cv*/ /*ref*/ noexcept(/*noex*/);  // (since C++23)
```

Invokes the stored callable target with the parameters `args`. The `/*cv*/`,
`/*ref*/`, and `/*noex*/` parts of `operator()` are identical to those of the
template parameter of `std::move_only_function`.

Equivalent to `return std::invoke_r<R>(/*cv-ref-cast*/(f),
std::forward<Args>(args)...);`, where `f` is a cv-unqualified lvalue that
denotes the target object of `*this`, and `/*cv-ref-cast*/(f)` is equivalent to:

- `f` if cv ref is either empty or `&`, or
- `std::as_const(f)` if cv ref is either `const` or `const &`, or
- `std::move(f)` if cv ref is `&&`, or
- `std::move(std::as_const(f))` if cv ref is `const &&`.

The behavior is undefined if `*this` is empty.

### Parameters

- **args** — parameters to pass to the stored callable target

### Return value

`std::invoke_r<R>(/*cv-ref-cast*/(f), std::forward<Args>(args)...)`.

### Exceptions

Propagates the exception thrown by the underlying function call.

### Example

The following example shows how `std::move_only_function` can passed to other
functions by value. Also, it shows how `std::move_only_function` can store
lambdas.

```cpp
#include <iostream>
#include <functional>

void call(std::move_only_function<int() const> f)  // can be passed by value
{
    std::cout << f() << '\n';
}

int normal_function()
{
    return 42;
}

int main()
{
    int n = 1;
    auto lambda = [&n](){ return n; };
    std::move_only_function<int() const> f = lambda;
    call(std::move(f));

    n = 2;
    call(lambda);

    f = normal_function;
    call(std::move(f));
}
```

Output:

```text
1
2
42
```

### See also

- **operator()** — invokes the target (public member function of
  `std::function<R(Args...)>`)
- **operator()** — calls the stored function (public member function of
  `std::reference_wrapper<T>`)
- **invokeinvoke_r (C++17)(C++23)** — invokes any Callable object with given
  arguments and possibility to specify return type(since C++23) (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/move_only_function/operator()*
