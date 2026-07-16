# std::atomic<T>::atomic

```cpp
atomic() noexcept = default;  // (since C++11) (until C++20)
constexpr atomic() noexcept(std::is_nothrow_default_constructible_v<T>);  // (since C++20)
constexpr atomic( T desired ) noexcept;  // (2) (since C++11)
atomic( const atomic& ) = delete;  // (3) (since C++11)
```

Constructs a new atomic variable.

1) The default constructor is trivial: no initialization takes place other than
zero initialization of static and thread-local objects. `std::atomic_init` may
be used to complete initialization.
*(until C++20)*

1) Value-initializes the underlying object (i.e. with `T()`). The initialization
is not atomic. The program is ill-formed if `std::is_default_constructible_v<T>`
is `false`.
*(since C++20)*

2) Initializes the underlying object with `desired`. The initialization is not
   atomic.

3) Atomic variables are not CopyConstructible.

### Parameters

- **desired** — value to initialize with

### Notes

The default-initialized std::atomic<T> does not contain a `T` object, and its
only valid uses are destruction and initialization by `std::atomic_init`, see
LWG issue 2334.
*(until C++20)*

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/atomic*
