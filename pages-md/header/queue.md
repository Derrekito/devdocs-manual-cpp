# Standard library header <queue>

This header is part of the containers library.

**Includes**

- **<compare> (C++20)** — Three-way comparison operator support
- **<initializer_list> (C++11)** — `std::initializer_list` class template

**Classes**

- **queue** — adapts a container to provide queue (FIFO data structure) (class
  template)
- **priority_queue** — adapts a container to provide priority queue (class
  template)
- **std::uses_allocator<std::queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)
- **std::uses_allocator<std::priority_queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

**Functions**

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++20)** — lexicographically compares the values of two `queues` (function
  template)
- **std::swap(std::queue) (C++11)** — specializes the `std::swap` algorithm
  (function template)
- **std::swap(std::priority_queue) (C++11)** — specializes the `std::swap`
  algorithm (function template)

### Synopsis

```cpp
#include <compare>
#include <initializer_list>

namespace std {
  // class template queue
  template<class T, class Container = deque<T>> class queue;

  template<class T, class Container>
    bool operator==(const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, class Container>
    bool operator!=(const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, class Container>
    bool operator< (const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, class Container>
    bool operator> (const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, class Container>
    bool operator<=(const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, class Container>
    bool operator>=(const queue<T, Container>& x, const queue<T, Container>& y);
  template<class T, three_way_comparable Container>
    compare_three_way_result_t<Container>
      operator<=>(const queue<T, Container>& x, const queue<T, Container>& y);

  template<class T, class Container>
    void swap(queue<T, Container>& x, queue<T, Container>& y)
      noexcept(noexcept(x.swap(y)));
  template<class T, class Container, class Alloc>
    struct uses_allocator<queue<T, Container>, Alloc>;

  // class template priority_queue
  template<class T, class Container = vector<T>,
           class Compare = less<typename Container::value_type>>
    class priority_queue;

  template<class T, class Container, class Compare>
    void swap(priority_queue<T, Container, Compare>& x,
              priority_queue<T, Container, Compare>& y) noexcept(noexcept(x.swap(y)));
  template<class T, class Container, class Compare, class Alloc>
    struct uses_allocator<priority_queue<T, Container, Compare>, Alloc>;
}
```

#### Class template `std::queue`

```cpp
namespace std {
  template<class T, class Container = deque<T>>
  class queue {
  public:
    using value_type      = typename Container::value_type;
    using reference       = typename Container::reference;
    using const_reference = typename Container::const_reference;
    using size_type       = typename Container::size_type;
    using container_type  =          Container;

  protected:
    Container c;

  public:
    queue() : queue(Container()) {}
    explicit queue(const Container&);
    explicit queue(Container&&);
    template<class InputIt> queue(InputIt first, InputIt last);
    template<container-compatible-range<T> R> queue(from_range_t, R&& rg);
    template<class Alloc> explicit queue(const Alloc&);
    template<class Alloc> queue(const Container&, const Alloc&);
    template<class Alloc> queue(Container&&, const Alloc&);
    template<class Alloc> queue(const queue&, const Alloc&);
    template<class Alloc> queue(queue&&, const Alloc&);
    template<class InputIt, class Alloc>
      queue(InputIt first, InputIt last, const Alloc&);
    template<container-compatible-range<T> R, class Alloc>
      queue(from_range_t, R&& rg, const Alloc&);

    [[nodiscard]] bool empty() const    { return c.empty(); }
    size_type         size()  const     { return c.size(); }
    reference         front()           { return c.front(); }
    const_reference   front() const     { return c.front(); }
    reference         back()            { return c.back(); }
    const_reference   back() const      { return c.back(); }
    void push(const value_type& x)      { c.push_back(x); }
    void push(value_type&& x)           { c.push_back(std::move(x)); }
    template<container-compatible-range<T> R> void push_range(R&& rg);
    template<class... Args>
      decltype(auto) emplace(Args&&... args)
        { return c.emplace_back(std::forward<Args>(args)...); }
    void pop()                          { c.pop_front(); }
    void swap(queue& q) noexcept(is_nothrow_swappable_v<Container>)
      { using std::swap; swap(c, q.c); }
  };

  template<class Container>
    queue(Container) -> queue<typename Container::value_type, Container>;

  template<class InputIt>
    queue(InputIt, InputIt) -> queue<__iter_value_type<InputIt>>;

  template<ranges::input_range R>
    queue(from_range_t, R&&) -> queue<ranges::range_value_t<R>>;

  template<class Container, class Allocator>
    queue(Container, Allocator) -> queue<typename Container::value_type, Container>;

  template<class InputIt, class Allocator>
    queue(InputIt, InputIt, Allocator)
      -> queue<__iter_value_type<InputIt>, deque<__iter_value_type<InputIt>,
               Allocator>>;

  template<ranges::input_range R, class Allocator>
    queue(from_range_t, R&&, Allocator)
      -> queue<ranges::range_value_t<R>, deque<ranges::range_value_t<R>, Allocator>>;

  template<class T, class Container, class Alloc>
    struct uses_allocator<queue<T, Container>, Alloc>
      : uses_allocator<Container, Alloc>::type { };
}
```

#### Class template `std::priority_queue`

```cpp
namespace std {
  template<class T, class Container = vector<T>,
           class Compare = less<typename Container::value_type>>
  class priority_queue {
  public:
    using value_type      = typename Container::value_type;
    using reference       = typename Container::reference;
    using const_reference = typename Container::const_reference;
    using size_type       = typename Container::size_type;
    using container_type  = Container;
    using value_compare   = Compare;

  protected:
    Container c;
    Compare comp;

  public:
    priority_queue() : priority_queue(Compare()) {}
    explicit priority_queue(const Compare& x) : priority_queue(x, Container()) {}
    priority_queue(const Compare& x, const Container&);
    priority_queue(const Compare& x, Container&&);
    template<class InputIt>
      priority_queue(InputIt first, InputIt last, const Compare& x = Compare());
    template<class InputIt>
      priority_queue(InputIt first, InputIt last, const Compare& x,
                     const Container&);
    template<class InputIt>
      priority_queue(InputIt first, InputIt last, const Compare& x,
                     Container&&);
    template<container-compatible-range<T> R>
      priority_queue(from_range_t, R&& rg, const Compare& x = Compare());
    template<class Alloc> explicit priority_queue(const Alloc&);
    template<class Alloc> priority_queue(const Compare&, const Alloc&);
    template<class Alloc> priority_queue(const Compare&, const Container&, const Alloc&);
    template<class Alloc> priority_queue(const Compare&, Container&&, const Alloc&);
    template<class Alloc> priority_queue(const priority_queue&, const Alloc&);
    template<class Alloc> priority_queue(priority_queue&&, const Alloc&);
    template<class InputIt, class Alloc>
      priority_queue(InputIt, InputIt, const Alloc&);
    template<class InputIt, class Alloc>
      priority_queue(InputIt, InputIt, const Compare&, const Alloc&);
    template<class InputIt, class Alloc>
      priority_queue(InputIt, InputIt, const Compare&, const Container&,
                     const Alloc&);
    template<class InputIt, class Alloc>
      priority_queue(InputIt, InputIt, const Compare&, Container&&, const Alloc&);
    template<container-compatible-range<T> R, class Alloc>
      priority_queue(from_range_t, R&& rg, const Compare&, const Alloc&);
    template<container-compatible-range<T> R, class Alloc>
      priority_queue(from_range_t, R&& rg, const Alloc&);

    [[nodiscard]] bool empty() const { return c.empty(); }
    size_type size()  const          { return c.size(); }
    const_reference   top() const    { return c.front(); }
    void push(const value_type& x);
    void push(value_type&& x);
    template<container-compatible-range<T> R>
      void push_range(R&& rg);
    template<class... Args> void emplace(Args&&... args);
    void pop();
    void swap(priority_queue& q) noexcept(is_nothrow_swappable_v<Container> &&
                                          is_nothrow_swappable_v<Compare>)
      { using std::swap; swap(c, q.c); swap(comp, q.comp); }
  };

  template<class Compare, class Container>
    priority_queue(Compare, Container)
      -> priority_queue<typename Container::value_type, Container, Compare>;

  template<class InputIt,
           class Compare = less<__iter_value_type<InputIt>>,
           class Container = vector<__iter_value_type<InputIt>>>
    priority_queue(InputIt, InputIt, Compare = Compare(), Container = Container())
      -> priority_queue<__iter_value_type<InputIt>, Container, Compare>;

  template<ranges::input_range R, class Compare = less<ranges::range_value_t<R>>>
    priority_queue(from_range_t, R&&, Compare = Compare())
      -> priority_queue<ranges::range_value_t<R>, vector<ranges::range_value_t<R>>,
                        Compare>;

  template<class Compare, class Container, class Allocator>
    priority_queue(Compare, Container, Allocator)
      -> priority_queue<typename Container::value_type, Container, Compare>;

  template<class InputIt, class Allocator>
    priority_queue(InputIt, InputIt, Allocator)
      -> priority_queue<__iter_value_type<InputIt>,
                        vector<__iter_value_type<InputIt>, Allocator>,
                        less<__iter_value_type<InputIt>>>;

  template<class InputIt, class Compare, class Allocator>
    priority_queue(InputIt, InputIt, Compare, Allocator)
      -> priority_queue<__iter_value_type<InputIt>,
                        vector<__iter_value_type<InputIt>, Allocator>, Compare>;

  template<class InputIt, class Compare, class Container, class Allocator>
    priority_queue(InputIt, InputIt, Compare, Container, Allocator)
      -> priority_queue<typename Container::value_type, Container, Compare>;

  template<ranges::input_range R, class Compare, class Allocator>
    priority_queue(from_range_t, R&&, Compare, Allocator)
      -> priority_queue<ranges::range_value_t<R>,
                        vector<ranges::range_value_t<R>, Allocator>,
                        Compare>;

  template<ranges::input_range R, class Allocator>
    priority_queue(from_range_t, R&&, Allocator)
      -> priority_queue<ranges::range_value_t<R>,
                        vector<ranges::range_value_t<R>, Allocator>>;

  // no equality is provided

  template<class T, class Container, class Compare, class Alloc>
    struct uses_allocator<priority_queue<T, Container, Compare>, Alloc>
      : uses_allocator<Container, Alloc>::type { };
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/queue*
