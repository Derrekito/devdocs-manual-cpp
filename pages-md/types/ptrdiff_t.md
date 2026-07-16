# std::ptrdiff_t

```cpp
typedef /*implementation-defined*/ ptrdiff_t;
```

`std::ptrdiff_t` is the signed integer type of the result of subtracting two
pointers.

The bit width of `std::ptrdiff_t` is not less than 17.
*(since C++11)*

### Notes

`std::ptrdiff_t` is used for pointer arithmetic and array indexing, if negative
values are possible. Programs that use other types, such as `int`, may fail on,
e.g. 64-bit systems when the index exceeds `INT_MAX` or if it relies on 32-bit
modular arithmetic.

When working with the C++ container library, the proper type for the difference
between iterators is the member typedef `difference_type`, which is often
synonymous with `std::ptrdiff_t`.

Only pointers to elements of the same array (including the pointer one past the
end of the array) may be subtracted from each other.

If an array is so large (greater than `PTRDIFF_MAX` elements, but less than
`SIZE_MAX` bytes), that the difference between two pointers may not be
representable as `std::ptrdiff_t`, the result of subtracting two such pointers
is undefined.

For char arrays shorter than `PTRDIFF_MAX`, `std::ptrdiff_t` acts as the signed
counterpart of `std::size_t`: it can store the size of the array of any type and
is, on most platforms, synonymous with `std::intptr_t`.

### Example

```cpp
#include <cstddef>
#include <iostream>

int main()
{
    const std::size_t N = 10;
    int* a = new int[N];
    int* end = a + N;
    for (std::ptrdiff_t i = N; i > 0; --i)
        std::cout << (*(end - i) = i) << ' ';
    std::cout << '\n';
    delete[] a;
}
```

Output:

```text
10 9 8 7 6 5 4 3 2 1
```

### See also

- **size_t** — unsigned integer type returned by the `sizeof` operator (typedef)
- **offsetof** — byte offset from the beginning of a standard-layout type to
  specified member (function macro)

**C documentation for `ptrdiff_t`**

---
*Source: https://en.cppreference.com/w/cpp/types/ptrdiff_t*
