# std::ostream_iterator<T,CharT,Traits>::operator=

```cpp
ostream_iterator& operator=( const T& value );
```

Inserts `value` into the associated stream, then inserts the delimiter, if one
was specified at construction time.

If `out_stream` is a pointer to the associated `std::basic_ostream` and `delim`
is the delimiter specified at the construction of this object, then the effect
is equivalent to

`*out_stream << value; if (delim != 0) *out_stream << delim; return *this;`

### Parameters

- **value** — the object to insert

### Return value

`*this`

### Notes

`T` can be any class with a user-defined `operator<<`.

### Example

```cpp
#include <iostream>
#include <iterator>

int main()
{
    std::ostream_iterator<int> i1(std::cout, ", ");
    *i1++ = 1; // usual form, used by standard algorithms
    *++i1 = 2;
    i1 = 3; // neither * nor ++ are necessary
    std::ostream_iterator<double> i2(std::cout);
    i2 = 3.14;
    std::cout << '\n';
}
```

Output:

```text
1, 2, 3, 3.14
```

---
*Source: https://en.cppreference.com/w/cpp/iterator/ostream_iterator/operator%3D*
