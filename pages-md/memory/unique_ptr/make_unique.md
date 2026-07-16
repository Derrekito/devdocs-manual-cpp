# std::make_unique, std::make_unique_for_overwrite

```cpp
template< class T, class... Args >
unique_ptr<T> make_unique( Args&&... args );  // (since C++14) (until C++23) (only for non-array types)
template< class T, class... Args >
constexpr unique_ptr<T> make_unique( Args&&... args );  // (since C++23) (only for non-array types)
template< class T >
unique_ptr<T> make_unique( std::size_t size );  // (since C++14) (until C++23) (only for array types with unknown bound)
template< class T >
constexpr unique_ptr<T> make_unique( std::size_t size );  // (since C++23) (only for array types with unknown bound)
template< class T, class... Args >
/* unspecified */ make_unique( Args&&... args ) = delete;  // (3) (since C++14) (only for array types with known bound)
template< class T >
unique_ptr<T> make_unique_for_overwrite();  // (since C++20) (until C++23) (only for non-array types)
template< class T >
constexpr unique_ptr<T> make_unique_for_overwrite();  // (since C++23) (only for non-array types)
template< class T >
unique_ptr<T> make_unique_for_overwrite( std::size_t size );  // (since C++20) (until C++23) (only for array types with unknown bound)
template< class T >
constexpr unique_ptr<T> make_unique_for_overwrite( std::size_t size );  // (since C++23) (only for array types with unknown bound)
template< class T, class... Args >
/* unspecified */ make_unique_for_overwrite( Args&&... args ) = delete;  // (6) (since C++20) (only for array types with known bound)
```

Constructs an object of type `T` and wraps it in a `std::unique_ptr`.

1) Constructs a non-array type `T`. The arguments `args` are passed to the
   constructor of `T`. This overload participates in overload resolution only if
   `T` is not an array type. The function is equivalent to: unique_ptr<T>(new
   T(std::forward<Args>(args)...))

2) Constructs an array of the given dynamic size. The array elements are
   value-initialized. This overload participates in overload resolution only if
   `T` is an array of unknown bound. The function is equivalent to:
   unique_ptr<T>(new std::remove_extent_t<T>[size]())

3,6) Construction of arrays of known bound is disallowed.

4) Same as (1), except that the object is default-initialized. This overload
   participates in overload resolution only if `T` is not an array type. The
   function is equivalent to: unique_ptr<T>(new T)

5) Same as (2), except that the array is default-initialized. This overload
   participates in overload resolution only if `T` is an array of unknown bound.
   The function is equivalent to: unique_ptr<T>(new
   std::remove_extent_t<T>[size])

### Parameters

- **args** — list of arguments with which an instance of `T` will be constructed
- **size** — the length of the array to construct

### Return value

`std::unique_ptr` of an instance of type `T`.

### Exceptions

May throw `std::bad_alloc` or any exception thrown by the constructor of `T`. If
an exception is thrown, this function has no effect.

### Possible Implementation

```cpp
// C++14 make_unique
namespace detail
{
    template<class>
    constexpr bool is_unbounded_array_v = false;
    template<class T>
    constexpr bool is_unbounded_array_v<T[]> = true;

    template<class>
    constexpr bool is_bounded_array_v = false;
    template<class T, std::size_t N>
    constexpr bool is_bounded_array_v<T[N]> = true;
} // namespace detail

template<class T, class... Args>
std::enable_if_t<!std::is_array<T>::value, std::unique_ptr<T>>
make_unique(Args&&... args)
{
    return std::unique_ptr<T>(new T(std::forward<Args>(args)...));
}

template<class T>
std::enable_if_t<detail::is_unbounded_array_v<T>, std::unique_ptr<T>>
make_unique(std::size_t n)
{
    return std::unique_ptr<T>(new std::remove_extent_t<T>[n]());
}

template<class T, class... Args>
std::enable_if_t<detail::is_bounded_array_v<T>> make_unique(Args&&...) = delete;
```

```cpp
// C++20 make_unique_for_overwrite
template<class T>
    requires (!std::is_array_v<T>)
std::unique_ptr<T> make_unique_for_overwrite()
{
    return std::unique_ptr<T>(new T);
}

template<class T>
    requires std::is_unbounded_array_v<T>
std::unique_ptr<T> make_unique_for_overwrite(std::size_t n)
{
    return std::unique_ptr<T>(new std::remove_extent_t<T>[n]);
}

template<class T, class... Args>
    requires std::is_bounded_array_v<T>
void make_unique_for_overwrite(Args&&...) = delete;
```

### Notes

Unlike `std::make_shared` (which has `std::allocate_shared`), `std::make_unique`
does not have an allocator-aware counterpart. `allocate_unique` proposed in
P0211 would be required to invent the deleter type `D` for the
`std::unique_ptr<T,D>` it returns which would contain an allocator object and
invoke both `destroy` and `deallocate` in its `operator()`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_make_unique` | 201304L | (C++14) | `std::make_unique`; overload (1)
  `__cpp_lib_smart_ptr_for_overwrite` | 202002L | (C++20) | Smart pointer
      creation with default initialization
      (`std::allocate_shared_for_overwrite`, `std::make_shared_for_overwrite`,
      `std::make_unique_for_overwrite`); overloads (4-6)
  `__cpp_lib_constexpr_memory` | 202202L | (C++23) | `constexpr` for overloads
      (1,2,4,5)

### Example

```cpp
#include <cstddef>
#include <iomanip>
#include <iostream>
#include <memory>
#include <utility>

struct Vec3
{
    int x, y, z;

    // Following constructor is no longer needed since C++20.
    Vec3(int x = 0, int y = 0, int z = 0) noexcept : x(x), y(y), z(z) {}

    friend std::ostream& operator<<(std::ostream& os, const Vec3& v)
    {
        return os << "{ x=" << v.x << ", y=" << v.y << ", z=" << v.z << " }";
    }
};

// Output Fibonacci numbers to an output iterator.
template<typename OutputIt>
OutputIt fibonacci(OutputIt first, OutputIt last)
{
    for (int a = 0, b = 1; first != last; ++first)
    {
        *first = b;
        b += std::exchange(a, b);
    }
    return first;
}

int main()
{
    // Use the default constructor.
    std::unique_ptr<Vec3> v1 = std::make_unique<Vec3>();
    // Use the constructor that matches these arguments.
    std::unique_ptr<Vec3> v2 = std::make_unique<Vec3>(0, 1, 2);
    // Create a unique_ptr to an array of 5 elements.
    std::unique_ptr<Vec3[]> v3 = std::make_unique<Vec3[]>(5);

    // Create a unique_ptr to an uninitialized array of 10 integers,
    // then populate it with Fibonacci numbers.
    std::unique_ptr<int[]> i1 = std::make_unique_for_overwrite<int[]>(10);
    fibonacci(i1.get(), i1.get() + 10);

    std::cout << "make_unique<Vec3>():      " << *v1 << '\n'
              << "make_unique<Vec3>(0,1,2): " << *v2 << '\n'
              << "make_unique<Vec3[]>(5):   ";
    for (std::size_t i = 0; i < 5; ++i)
        std::cout << std::setw(i ? 30 : 0) << v3[i] << '\n';
    std::cout << '\n';

    std::cout << "make_unique_for_overwrite<int[]>(10), fibonacci(...): [" << i1[0];
    for (std::size_t i = 1; i < 10; ++i)
        std::cout << ", " << i1[i];
    std::cout << "]\n";
}
```

Output:

```text
make_unique<Vec3>():      { x=0, y=0, z=0 }
make_unique<Vec3>(0,1,2): { x=0, y=1, z=2 }
make_unique<Vec3[]>(5):   { x=0, y=0, z=0 }
                          { x=0, y=0, z=0 }
                          { x=0, y=0, z=0 }
                          { x=0, y=0, z=0 }
                          { x=0, y=0, z=0 }

make_unique_for_overwrite<int[]>(10), fibonacci(...): [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
```

### See also

- **(constructor)** — constructs a new `unique_ptr` (public member function)
- **make_sharedmake_shared_for_overwrite (C++20)** — creates a shared pointer
  that manages a new object (function template)

---
*Source: https://en.cppreference.com/w/cpp/memory/unique_ptr/make_unique*
