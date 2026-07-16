# std::coroutine_handle<Promise>::from_promise

```cpp
static coroutine_handle from_promise( Promise& p );  // (since C++20)
```

Creates a `coroutine_handle` from the promise object of a coroutine. The created
`coroutine_handle` refers the coroutine, and `promise()` returns a reference to
`p`.

The behavior is undefined if `p` is not a reference to a promise object. This
function is only provided for the primary template, i.e. specializations
`std::coroutine_handle<>` and
`std::coroutine_handle<std::noop_coroutine_promise>` do not have this function.

### Parameters

- **p** — promise object of a coroutine to refer

### Return value

A `coroutine_handle` referring the given coroutine.

### Example

### See also

- **(constructor) (C++20)** — constructs a `coroutine_handle` object (public
  member function)
- **from_address [static] (C++20)** — imports a coroutine from a pointer (public
  static member function)
- **noop_coroutine (C++20)** — creates a coroutine handle that has no observable
  effects when resumed or destroyed (function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/coroutine_handle/from_promise*
