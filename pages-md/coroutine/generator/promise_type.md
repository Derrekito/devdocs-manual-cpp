# std::generator<Ref,V,Allocator>::promise_type

```cpp
class generator<Ref, V, Allocator>::promise_type;  // (since C++23)
```

The promise type of `std::generator`.

### Data members

- **`value_` (private)** — A pointer of type
  `std::add_pointer_t<std::generator::yielded>` to the yielded value. Default
  value is nullptr. (exposition-only member object*)
- **`except_` (private)** — A pointer of type `std::exception_ptr` to an
  exception object. (exposition-only member object*)

### Member functions

- **(constructor) (implicitly declared)** — constructs the `promise_type` object
  (public member function)
- **(destructor) (implicitly declared)** — destroys the `promise_type` object
  (public member function)
- **get_return_object** — issues the generator object (public member function)
- **initial_suspend** — issues an awaiter for initial suspend point (public
  member function)
- **final_suspend** — issues an awaiter for final suspend point (public member
  function)
- **yield_value** — processes the object obtained from co_yield (public member
  function)
- **await_transform [deleted]** — maps the object obtained from co_await to an
  awaiter (public member function)
- **return_void** — handles `co_return;` or the exit out of coroutine's body
  (public member function)
- **unhandled_exception** — processes exceptions that leaked from the
  coroutine's body (public member function)
- **operator new** — allocates memory using `Allocator` (public member function)
- **operator delete** — deallocates memory previously obtained from `operator
  new` (public member function)

### Example

### See also

- **noop_coroutine_promise (C++20)** — used for coroutines with no observable
  effects (class)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator/promise_type*
