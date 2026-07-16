# std::start_lifetime_as, std::start_lifetime_as_array

```cpp
start_lifetime_as
template< class T >
T* start_lifetime_as( void* p ) noexcept;  // (1) (since C++23)
template< class T >
const T* start_lifetime_as( const void* p ) noexcept;  // (2) (since C++23)
template< class T >
volatile T* start_lifetime_as( volatile void* p ) noexcept;  // (3) (since C++23)
template< class T >
const volatile T* start_lifetime_as( const volatile void* p ) noexcept;  // (4) (since C++23)
start_lifetime_as_array
template< class T >
T* start_lifetime_as_array( void* p, std::size_t n ) noexcept;  // (5) (since C++23)
template< class T >
const T* start_lifetime_as_array( const void* p,
                                  std::size_t n ) noexcept;  // (6) (since C++23)
template< class T >
volatile T* start_lifetime_as_array( volatile void* p,
                                     std::size_t n ) noexcept;  // (7) (since C++23)
template< class T >
const volatile T* start_lifetime_as_array( const volatile void* p,
                                           std::size_t n ) noexcept;  // (8) (since C++23)
```

1-4) Implicitly creates a complete object of type `T` (whose address is `p`) and
objects nested within it. The value of each created object `obj` of
TriviallyCopyable type `U` is determined in the same manner as for a call to
`std::bit_cast<U>(E)` except that the storage is not actually accessed, where
`E` is the lvalue of type `U` denoting `obj`. Otherwise, the values of such
created objects are unspecified.

- `T` shall be an ImplicitLifetimeType and shall be a complete type. Otherwise,
  the program is ill-formed.
- The behavior is undefined if:
- Note that the unspecified value can be indeterminate.

5-8) Implicitly creates an array with element type `T` and length `n`. To be
precise, if `n > 0` is `true`, it is equivalent to
`std::start_lifetime_as<U>(p)` where `U` is the type "array of `n` `T`s".
Otherwise, the function has no effects.

- `T` shall be a complete type. Otherwise, the program is ill-formed.
- The behavior is undefined if:

### Parameters

- **p** — the address of the region consisting objects
- **n** — the number of the element of the array to be created

### Return value

1-4) A pointer to the complete object as described above.

5-8) A pointer to the first element of the created array, if any; otherwise, a
   pointer that compares equal to `p`.

### Notes

`new (void_ptr) unsigned char[size]` or `new (void_ptr) std::byte[size]` works
as an untyped version of `std::start_lifetime_as`, but it does not keep the
object representation.

`std::start_lifetime_as` handles non-array types as well as arrays of known
bound, while `std::start_lifetime_as_array` handles arrays of unknown bound.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_start_lifetime_as` | 202207L | (C++23) | Explicit lifetime
      management

### Example

```cpp
#include <complex>
#include <iostream>
#include <memory>

int main()
{
    alignas(std::complex<float>) unsigned char network_data[sizeof(std::complex<float>)]{
        0xcd, 0xcc, 0xcc, 0x3d, 0xcd, 0xcc, 0x4c, 0x3e
    };

//  auto d = *reinterpret_cast<std::complex<float>*>(network_data);
//  std::cout << d << '\n'; // UB: network_data does not point to a complex<float>

//  auto d = *std::launder(reinterpret_cast<std::complex<float>*>(network_data));
//  std::cout << d << '\n'; // Possible UB, related to CWG1997:
//      the implicitly created complex<float> may hold indeterminate value

    auto d = *std::start_lifetime_as<std::complex<float>>(network_data);
    std::cout << d << '\n'; // OK
}
```

Possible output:

```text
(0.1,0.2)
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **bit_cast (C++20)** — reinterpret the object representation of one type as
  that of another (function template)
- **as_bytesas_writable_bytes (C++20)** — converts a `span` into a view of its
  underlying bytes (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/start_lifetime_as*
