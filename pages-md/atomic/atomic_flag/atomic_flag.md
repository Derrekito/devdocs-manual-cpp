# std::atomic_flag::atomic_flag

```cpp
atomic_flag() noexcept = default;  // (since C++11) (until C++20)
constexpr atomic_flag() noexcept;  // (since C++20)
atomic_flag( const atomic_flag& ) = delete;  // (2) (since C++11)
```

Constructs a new `std::atomic_flag`.

1) Trivial default constructor, initializes `std::atomic_flag` to unspecified
state.
*(until C++20)*

1) Initializes `std::atomic_flag` to clear state.
*(since C++20)*

2) The copy constructor is deleted; `std::atomic_flag` is not copyable.

In addition, `std::atomic_flag` can be value-initialized to clear state with the
expression `ATOMIC_FLAG_INIT`. For an `atomic_flag` with static storage
duration, this guarantees static initialization: the flag can be used in
constructors of static objects.

### See also

- **ATOMIC_FLAG_INIT (C++11)** — initializes an `std::atomic_flag` to `false`
  (macro constant)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag/atomic_flag*
