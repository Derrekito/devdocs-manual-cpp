# Standard library header <hazard_pointer> (C++26)

This header is part of the thread support library.

**Classes**

- **hazard_pointer_obj_base (C++26)** — allows an object to be
  hazard-protectable (class template)
- **hazard_pointer (C++26)** — single-writer multi-reader pointer that can be
  owned by at most one thread at any point of time (class)

**Functions**

- **make_hazard_pointer (C++26)** — constructs a hazard pointer (function)
- **std::swap(std::hazard_pointer) (C++26)** — specializes the `std::swap`
  algorithm (function template)

### Synopsis

```cpp
namespace std {
  // hazard_pointer_obj_base
  template<class T, class D = default_delete<T>> class hazard_pointer_obj_base;

  // hazard_pointer
  class hazard_pointer;

  // make_hazard_pointer
  hazard_pointer make_hazard_pointer();
  void swap(hazard_pointer&, hazard_pointer&) noexcept;
}
```

#### Class template `std::hazard_pointer_obj_base`

```cpp
namespace std {
  template<class T, class D = default_delete<T>>
  class hazard_pointer_obj_base {
  public:
    void retire(D d = D()) noexcept;
  protected:
    hazard_pointer_obj_base() = default;
    hazard_pointer_obj_base(const hazard_pointer_obj_base&) = default;
    hazard_pointer_obj_base(hazard_pointer_obj_base&&) = default;
    hazard_pointer_obj_base& operator=(const hazard_pointer_obj_base&) = default;
    hazard_pointer_obj_base& operator=(hazard_pointer_obj_base&&) = default;
    ~hazard_pointer_obj_base() = default;
  private:
    D deleter;      // exposition only
  };
}
```

#### Class `std::hazard_pointer`

```cpp
namespace std {
  class hazard_pointer {
  public:
    hazard_pointer() noexcept;
    hazard_pointer(hazard_pointer&&) noexcept;
    hazard_pointer& operator=(hazard_pointer&&) noexcept;
    ~hazard_pointer();

    [[nodiscard]] bool empty() const noexcept;
    template<class T> T* protect(const atomic<T*>& src) noexcept;
    template<class T> bool try_protect(T*& ptr, const atomic<T*>& src) noexcept;
    template<class T> void reset_protection(const T* ptr) noexcept;
    void reset_protection(nullptr_t = nullptr) noexcept;
    void swap(hazard_pointer&) noexcept;
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/hazard_pointer*
