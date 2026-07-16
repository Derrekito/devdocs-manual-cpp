# std::generator<Ref,V,Allocator>::promise_type::unhandled_exception

```cpp
void unhandled_exception();  // (since C++23)
```

Let `x` be some generator object.

If a handle referring to the coroutine whose promise object is `*this` is at the
top of `*active_` of `x`:

- If the handle referring to the coroutine whose promise object is `*this` is
  the only element of `x.*active_`, equivalent to `throw`.
- Otherwise, assigns `std::current_exception()` to `except_`.
- Otherwise, the behavior is undefined.

### Parameters

(none)

### Return value

(none)

### Exceptions

May throw.

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type/unhandled_exception*
