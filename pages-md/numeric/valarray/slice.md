# std::slice

```cpp
class slice;
```

`std::slice` is the selector class that identifies a subset of `std::valarray`
similar to BLAS slice. An object of type `std::slice` holds three values: the
starting index, the stride, and the total number of values in the subset.
Objects of type `std::slice` can be used as indexes with valarray's operator[].

### Member functions

- ****(constructor)**** — constructs a slice (public member function)
- ****startsizestride**** — returns the parameters of the slice (public member
  function)

## std::slice::slice

```cpp
slice()  // (1)
slice( std::size_t start, std::size_t size, std::size_t stride );  // (2)
slice( const slice& other );  // (3)
```

Constructs a new slice.

1) Default constructor. Equivalent to `slice(0, 0, 0)`. This constructor exists
   only to allow construction of arrays of slices.

2) Constructs a new slice with parameters `start`, `size`, `stride`. This slice
   will refer to `size` number of elements, each with the position:

   start + 0 * stride

   start + 1 * stride

   ...

   start + (size - 1) * stride

3) Constructs a copy of `other`.

### Parameters

- **start** — the position of the first element
- **size** — the number of elements in the slice
- **stride** — the number of positions between successive elements in the slice
- **other** — another slice to copy

## std::slice::start, size, stride

```cpp
std::size_t start() const;  // (1)
std::size_t size() const;  // (2)
std::size_t stride() const;  // (3)
```

Returns the parameters passed to the slice on construction - start, size and
stride respectively.

### Parameters

(none)

### Return value

The parameters of the slice -- start, size and stride respectively.

### Complexity

Constant.

### Non-member functions

- ****operator==(std::slice)** (C++20)** — checks if two slices are equal
  (function)

## operator==(std::slice)

```cpp
friend bool operator==( const slice& lhs, const slice& rhs );  // (since C++20)
```

Checks if the parameters of `lhs` and `rhs` - start, size and stride are equal
respectively.

This function is not visible to ordinary unqualified or qualified lookup, and
can only be found by argument-dependent lookup when `std::slice` is an
associated class of the arguments.

The `!=` operator is synthesized from `operator==`.

### Parameters

- **lhs, rhs** — slices to compare

### Return value

`lhs.start() == rhs.start() && lhs.size() == rhs.size() && lhs.stride() ==
rhs.stride()`

### Example

Barebones valarray-backed Matrix class with a trace calculating function.

```cpp
#include <iostream>
#include <valarray>

class Matrix
{
    std::valarray<int> data;
    int dim;
public:
    Matrix(int r, int c) : data(r*c), dim(c) {}
    int& operator()(int r, int c) { return data[r * dim + c]; }
    int trace() const { return data[std::slice(0, dim, dim + 1)].sum(); }
};

int main()
{
    Matrix m(3, 3);
    int n = 0;
    for (int r = 0; r < 3; ++r)
       for (int c = 0; c < 3; ++c)
           m(r, c) = ++n;
    std::cout << "Trace of the matrix (1,2,3) (4,5,6) (7,8,9) is " << m.trace() << '\n';
}
```

Output:

```text
Trace of the matrix (1,2,3) (4,5,6) (7,8,9) is 15
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 543 | C++98 | it was unclear whether a default constructed slice is usable
      | it is usable (as an empty subset)

### See also

- **operator[]** — get/set valarray element, slice, or mask (public member
  function)
- **gslice** — generalized slice of a valarray: starting index, set of lengths,
  set of strides (class)
- **slice_array** — proxy to a subset of a valarray after applying a slice
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/valarray/slice*
