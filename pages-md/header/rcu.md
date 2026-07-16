# Standard library header <rcu> (C++26)

This header is part of the thread support library.

**Classes**

- **rcu_obj_base (C++26)** — allows an object to be protected by RCU (class
  template)
- **rcu_domain (C++26)** — provides regions of RCU protection (class)

**Functions**

- **rcu_default_domain (C++26)** — returns a reference to a static-duration
  object of type `std::rcu_domain` (function)
- **rcu_synchronize (C++26)** — blocks until a protection region unlocks on a
  RCU domain (function)
- **rcu_barrier (C++26)** — may evaluate scheduled operations on a RCU domain
  and blocks until all preceding evaluations are complete (function)
- **rcu_retire (C++26)** — schedules the evaluation of a specified function on a
  RCU domain, potentially allocating memory, and invoking scheduled evaluations
  (function template)

### Synopsis

```cpp
namespace std {
  template<class T, class D = default_delete<T>> class rcu_obj_base;

  class rcu_domain;

  rcu_domain& rcu_default_domain() noexcept;
  void rcu_synchronize(rcu_domain& dom = rcu_default_domain()) noexcept;
  void rcu_barrier(rcu_domain& dom = rcu_default_domain()) noexcept;
  template<class T, class D = default_delete<T>>
    void rcu_retire(T* p, D d = D(), rcu_domain& dom = rcu_default_domain());
}
```

#### Class template `std::rcu_obj_base`

```cpp
namespace std {
  template<class T, class D = default_delete<T>>
  class rcu_obj_base {
  public:
    void retire(D d = D(), rcu_domain& dom = rcu_default_domain()) noexcept;
  protected:
    rcu_obj_base() = default;
    rcu_obj_base(const rcu_obj_base&) = default;
    rcu_obj_base(rcu_obj_base&&) = default;
    rcu_obj_base& operator=(const rcu_obj_base&) = default;
    rcu_obj_base& operator=(rcu_obj_base&&) = default;
    ~rcu_obj_base() = default;
  private:
    D deleter;            // exposition only
  };
}
```

#### Class `std::rcu_domain`

```cpp
namespace std {
  class rcu_domain {
  public:
    rcu_domain(const rcu_domain&) = delete;
    rcu_domain& operator=(const rcu_domain&) = delete;

    void lock() noexcept;
    bool try_lock() noexcept;
    void unlock() noexcept;
  };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/rcu*
