# std::function_ref::operator()

```cpp
R operator()( Args... args ) const noexcept(/*noex*/);  // (since C++26)
```

Invokes the stored `thunk-ptr` with `bound-entity` as its first parameter and
the rest of the parameters `args`. The `/*noex*/` part of `operator()` is
identical to those of the template parameter of `std::function_ref`.

Equivalent to `return thunk-ptr(bound-entity, std::forward<Args>(args)...);`.

### Parameters

- **args** — rest parameters to pass to the stored `thunk-ptr`

### Return value

`thunk-ptr(bound-entity, std::forward<Args>(args)...)`.

### Exceptions

Propagates the exception thrown by the underlying function call.

### Example

### See also

- **operator()** — invokes the target (public member function of
  `std::function<R(Args...)>`)
- **operator()** — calls the stored function (public member function of
  `std::reference_wrapper<T>`)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/function_ref/operator()*
