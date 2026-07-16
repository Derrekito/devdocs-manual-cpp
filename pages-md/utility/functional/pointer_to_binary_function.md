# std::pointer_to_binary_function

```cpp
template<
    class Arg1,
    class Arg2,
    class Result
> class pointer_to_binary_function : public std::binary_function<Arg1, Arg2, Result>;  // (deprecated in C++11) (removed in C++17)
```

`std::pointer_to_binary_function` is a function object that acts as a wrapper
around a binary function.

### Member functions

- **(constructor)** — constructs a new `pointer_to_binary_function` object with
  the supplied function (public member function)
- **operator()** — calls the stored function (public member function)

## std::pointer_to_binary_function::pointer_to_binary_function

```cpp
explicit pointer_to_binary_function( Result (*f)(Arg1,Arg2) );
```

Constructs a `pointer_to_binary_function` function object with the stored
function `f`.

### Parameters

- **f** — pointer to a function to store

## std::pointer_to_binary_function::operator()

```cpp
Result operator()( Arg1 x1, Arg2 x2 ) const;
```

Calls the stored function.

### Parameters

- **x1, x2** — arguments to pass to the function

### Return value

The value returned by the called function.

### See also

- **pointer_to_unary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to unary function (class template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/pointer_to_binary_function*
