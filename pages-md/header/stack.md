# Standard library header <stack>

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **stack** — adapts a container to provide stack (LIFO data structure) (class
  template)
- **std::uses_allocator<std::stack> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++20)** — lexicographically compares the values of two `stacks` (function
  template)
- **std::swap(std::stack) (C++11)** — specializes the `std::swap` algorithm
  (function template)

### Synopsis

```cpp
#include <compare>
#include <initializer_list>

namespace std {
  // class template stack
  template<class T, class Container = deque<T>> class stack;

  template<class T, class Container>
    bool operator==(const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, class Container>
    bool operator!=(const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, class Container>
    bool operator< (const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, class Container>
    bool operator> (const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, class Container>
    bool operator<=(const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, class Container>
    bool operator>=(const stack<T, Container>& x, const stack<T, Container>& y);
  template<class T, three_way_comparable Container>
    compare_three_way_result_t<Container>
      operator<=>(const stack<T, Container>& x, const stack<T, Container>& y);

  template<class T, class Container>
    void swap(stack<T, Container>& x, stack<T, Container>& y)
      noexcept(noexcept(x.swap(y)));
  template<class T, class Container, class Alloc>
    struct uses_allocator<stack<T, Container>, Alloc>;
}
```

#### Class template `std::stack`

```cpp
namespace std {
  template<class T, class Container = deque<T>>
  class stack {
  public:
    using value_type      = typename Container::value_type;
    using reference       = typename Container::reference;
    using const_reference = typename Container::const_reference;
    using size_type       = typename Container::size_type;
    using container_type  = Container;

  protected:
    Container c;

  public:
    stack() : stack(Container()) {}
    explicit stack(const Container&);
    explicit stack(Container&&);
    template<class InputIt> stack(InputIt first, InputIt last);
    template<container-compatible-range<T> R> stack(from_range_t, R&& rg);
    template<class Alloc> explicit stack(const Alloc&);
    template<class Alloc> stack(const Container&, const Alloc&);
    template<class Alloc> stack(Container&&, const Alloc&);
    template<class Alloc> stack(const stack&, const Alloc&);
    template<class Alloc> stack(stack&&, const Alloc&);
    template<class InputIt, class Alloc>
      stack(InputIt first, InputIt last, const Alloc&);
    template<container-compatible-range<T> R, class Alloc>
      stack(from_range_t, R&& rg, const Alloc&);

    [[nodiscard]] bool empty() const    { return c.empty(); }
    size_type size()  const             { return c.size(); }
    reference         top()             { return c.back(); }
    const_reference   top() const       { return c.back(); }
    void push(const value_type& x)      { c.push_back(x); }
    void push(value_type&& x)           { c.push_back(std::move(x)); }
    template<container-compatible-range<T> R>
      void push_range(R&& rg);
    template<class... Args>
      decltype(auto) emplace(Args&&... args)
        { return c.emplace_back(std::forward<Args>(args)...); }
    void pop()                          { c.pop_back(); }
    void swap(stack& s) noexcept(is_nothrow_swappable_v<Container>)
      { using std::swap; swap(c, s.c); }
  };

  template<class Container>
    stack(Container) -> stack<typename Container::value_type, Container>;

  template<class InputIt>
    stack(InputIt, InputIt) -> stack<__iter_value_type<InputIt>>;

  template<ranges::input_range R>
    stack(from_range_t, R&&) -> stack<ranges::range_value_t<R>>;

  template<class Container, class Allocator>
    stack(Container, Allocator) -> stack<typename Container::value_type, Container>;

  template<class InputIt, class Allocator>
    stack(InputIt, InputIt, Allocator)
      -> stack<__iter_value_type<InputIt>, deque<__iter_value_type<InputIt>,
               Allocator>>;

  template<ranges::input_range R, class Allocator>
    stack(from_range_t, R&&, Allocator)
      -> stack<ranges::range_value_t<R>, deque<ranges::range_value_t<R>, Allocator>>;

  template<class T, class Container, class Alloc>
    struct uses_allocator<stack<T, Container>, Alloc>
      : uses_allocator<Container, Alloc>::type { };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/stack*
