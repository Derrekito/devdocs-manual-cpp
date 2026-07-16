# std::allocator<T>::deallocate

```cpp
void deallocate( T* p, std::size_t n );  // (constexpr since C++20)
```

Deallocates the storage referenced by the pointer `p`, which must be a pointer
obtained by an earlier call to `allocate()` or `allocate_at_least()`(since
C++23).

The argument `n` must be equal to the first argument of the call to `allocate()`
that originally produced `p`, or in the range `[``m``,``count``]` if `p` is
obtained from a call to `allocate_at_least(m)` which returned `{p, count}`(since
C++23); otherwise, the behavior is undefined.

Calls `::operator delete(void*)` or `::operator delete(void*,
std::align_val_t)`(since C++17), but it is unspecified when and how it is
called.

In evaluation of a constant expression, this function must deallocate storage
allocated within the evaluation of the same expression.
*(since C++20)*

### Parameters

- **p** — pointer obtained from `allocate()` or `allocate_at_least()`(since
  C++23)
- **n** — number of objects earlier passed to `allocate()`, or a number between
  requested and actually allocated number of objects via `allocate_at_least()`
  (may be equal to either bound)(since C++23)

### Return value

(none)

### Example

```cpp
#include <algorithm>
#include <cstddef>
#include <iostream>
#include <memory>
#include <string>

class S
{
    inline static int n{1};
    int m{};
    void pre() const { std::cout << "#" << m << std::string(m, ' '); }
public:
    S(int x) : m{n++} { pre(); std::cout << "S::S(" << x << ");\n"; }
    ~S() { pre(); std::cout << "S::~S();\n"; }
    void id() const { pre(); std::cout << "S::id();\n"; }
};

int main()
{
    constexpr std::size_t n{4};
    std::allocator<S> allocator;
    try
    {
        S* s = allocator.allocate(n); // may throw
        for (std::size_t i{}; i != n; ++i)
        {
        //  allocator.construct(&s[i], i + 42); // removed in C++20
            std::construct_at(&s[i], i + 42);   // since C++20
        }
        std::for_each_n(s, n, [](const auto& e) { e.id(); });
        std::destroy_n(s, n);
        allocator.deallocate(s, n);
    }
    catch (std::bad_array_new_length const& ex) { std::cout << ex.what() << '\n'; }
    catch (std::bad_alloc const& ex) { std::cout << ex.what() << '\n'; }
}
```

Output:

```text
#1 S::S(42);
#2  S::S(43);
#3   S::S(44);
#4    S::S(45);
#1 S::id();
#2  S::id();
#3   S::id();
#4    S::id();
#1 S::~S();
#2  S::~S();
#3   S::~S();
#4    S::~S();
```

### See also

- **allocate** — allocates uninitialized storage (public member function)
- **allocate_at_least (C++23)** — allocates uninitialized storage at least as
  large as requested size (public member function)
- **deallocate [static]** — deallocates storage using the allocator (public
  static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/deallocate*
