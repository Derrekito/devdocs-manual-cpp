# Standard library header <new>

This header is part of the dynamic memory management library, in particular
provides low level memory management features.

**Classes**

- **bad_alloc** — exception thrown when memory allocation fails (class)
- **bad_array_new_length (C++11)** — exception thrown on allocation of array
  with invalid length (class)
- **nothrow_t** — tag type used to select a non-throwing *allocation function*
  (class)
- **align_val_t (C++17)** — type used to pass alignment to alignment-aware
  allocation and deallocation functions (enum)
- **destroying_delete_t (C++20)** — tag type used to identify destroying-delete
  overloads of operator delete (class)

**Types**

- **new_handler** — function pointer type of the new handler (typedef)

**Constants**

- **nothrow** — an object of type `nothrow_t` used to select a non-throwing
  *allocation function* (constant)
- **destroying_delete (C++20)** — an object of type `std::destroying_delete_t`
  used to select destroying-delete overloads of `operator delete` (constant)
-
  **hardware_destructive_interference_sizehardware_constructive_interference_size
  (C++17)** — min offset to avoid false sharing max offset to promote true
  sharing (constant)

**Functions**

- **operator newoperator new[]** — allocation functions (function)
- **operator deleteoperator delete[]** — deallocation functions (function)
- **get_new_handler (C++11)** — obtains the current new handler (function)
- **set_new_handler** — registers a new handler (function)
- **launder (C++17)** — pointer optimization barrier (function template)

### Synopsis

```cpp
namespace std {
  // storage allocation errors
  class bad_alloc;
  class bad_array_new_length;

  struct destroying_delete_t {
    explicit destroying_delete_t() = default;
  };
  inline constexpr destroying_delete_t destroying_delete{};

  // global operator new control
  enum class align_val_t : size_t {};

  struct nothrow_t { explicit nothrow_t() = default; };
  extern const nothrow_t nothrow;

  using new_handler = void (*)();
  new_handler get_new_handler() noexcept;
  new_handler set_new_handler(new_handler new_p) noexcept;

  // pointer optimization barrier
  template<class T> [[nodiscard]] constexpr T* launder(T* p) noexcept;

  // hardware interference size
  inline constexpr size_t hardware_destructive_interference_size =
    /* implementation-defined */;
  inline constexpr size_t hardware_constructive_interference_size =
    /* implementation-defined */;
}

// storage allocation and deallocation
[[nodiscard]] void* operator new(std::size_t size);
[[nodiscard]] void* operator new(std::size_t size, std::align_val_t alignment);
[[nodiscard]] void* operator new(std::size_t size, const std::nothrow_t&) noexcept;
[[nodiscard]] void* operator new(std::size_t size, std::align_val_t alignment,
                                 const std::nothrow_t&) noexcept;

void  operator delete(void* ptr) noexcept;
void  operator delete(void* ptr, std::size_t size) noexcept;
void  operator delete(void* ptr, std::align_val_t alignment) noexcept;
void  operator delete(void* ptr, std::size_t size, std::align_val_t alignment) noexcept;
void  operator delete(void* ptr, const std::nothrow_t&) noexcept;
void  operator delete(void* ptr, std::align_val_t alignment,
                      const std::nothrow_t&) noexcept;

[[nodiscard]] void* operator new[](std::size_t size);
[[nodiscard]] void* operator new[](std::size_t size, std::align_val_t alignment);
[[nodiscard]] void* operator new[](std::size_t size, const std::nothrow_t&) noexcept;
[[nodiscard]] void* operator new[](std::size_t size, std::align_val_t alignment,
                                   const std::nothrow_t&) noexcept;

void  operator delete[](void* ptr) noexcept;
void  operator delete[](void* ptr, std::size_t size) noexcept;
void  operator delete[](void* ptr, std::align_val_t alignment) noexcept;
void  operator delete[](void* ptr, std::size_t size, std::align_val_t alignment) noexcept;
void  operator delete[](void* ptr, const std::nothrow_t&) noexcept;
void  operator delete[](void* ptr, std::align_val_t alignment,
                        const std::nothrow_t&) noexcept;

[[nodiscard]] void* operator new  (std::size_t size, void* ptr) noexcept;
[[nodiscard]] void* operator new[](std::size_t size, void* ptr) noexcept;
void  operator delete  (void* ptr, void*) noexcept;
void  operator delete[](void* ptr, void*) noexcept;
```

---
*Source: https://en.cppreference.com/w/cpp/header/new*
