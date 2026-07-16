# std::pointer_to_unary_function

```cpp
template<
    class Arg,
    class Result
> class pointer_to_unary_function : public std::unary_function<Arg, Result>;  // (deprecated in C++11) (removed in C++17)
```

`std::pointer_to_unary_function` is a function object that acts as a wrapper
around a unary function.

### Member functions

- **(constructor)** — constructs a new `pointer_to_unary_function` object with
  the supplied function (public member function)
- **operator()** — calls the stored function (public member function)

## std::pointer_to_unary_function::pointer_to_unary_function

```cpp
explicit pointer_to_unary_function( Result (*f)(Arg) );
```

Constructs a `pointer_to_unary_function` function object with the stored
function `f`.

### Parameters

- **f** — pointer to a function to store

## std::pointer_to_unary_function::operator()

```cpp
Result operator()( Arg x ) const;
```

Calls the stored function.

### Parameters

- **x** — argument to pass to the function

### Return value

The value returned by the called function.

### See also

- **pointer_to_binary_function (deprecated in C++11)(removed in C++17)** —
  adaptor-compatible wrapper for a pointer to binary function (class template)
- **ptr_fun (deprecated in C++11)(removed in C++17)** — creates an
  adaptor-compatible function object wrapper from a pointer to function
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/functional/pointer_to_unary_function*
