# std::chrono::duration_values

```cpp
template< class Rep >
struct duration_values;  // (since C++11)
```

The `std::chrono::duration_values` type defines three common durations:

- `std::chrono::duration_values::zero`
- `std::chrono::duration_values::min`
- `std::chrono::duration_values::max`

The zero, min, and max static member functions in `std::chrono::duration`
forward their work to these functions.

This type can be specialized if the representation `Rep` requires a specific
implementation to return these duration objects.

### Member functions

- **zero [static]** — returns a zero-length representation (public static member
  function)
- **min [static]** — returns the smallest possible representation (public static
  member function)
- **max [static]** — returns the largest possible representation (public static
  member function)

---
*Source: https://en.cppreference.com/w/cpp/chrono/duration_values*
