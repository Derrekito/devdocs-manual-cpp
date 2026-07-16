# std::priority_queue

```cpp
template<
    class T,
    class Container = std::vector<T>,
    class Compare = std::less<typename Container::value_type>
> class priority_queue;
```

The priority queue is a container adaptor that provides constant time lookup of
the largest (by default) element, at the expense of logarithmic insertion and
extraction.

A user-provided `Compare` can be supplied to change the ordering, e.g. using
`std::greater<T>` would cause the smallest element to appear as the `top()`.

Working with a `priority_queue` is similar to managing a heap in some random
access container, with the benefit of not being able to accidentally invalidate
the heap.

### Template parameters

- **T** — The type of the stored elements. The behavior is undefined if `T` is
  not the same type as `Container::value_type`.
- **Container** — The type of the underlying container to use to store the
  elements. The container must satisfy the requirements of SequenceContainer,
  and its iterators must satisfy the requirements of LegacyRandomAccessIterator.
  Additionally, it must provide the following functions with the usual
  semantics: `front()` `push_back()` `pop_back()`. The standard containers
  `std::vector` (including `std::vector<bool>`) and `std::deque` satisfy these
  requirements.
- **Compare** — A Compare type providing a strict weak ordering. Note that the
  Compare parameter is defined such that it returns `true` if its first argument
  comes *before* its second argument in a weak ordering. But because the
  priority queue outputs largest elements first, the elements that "come before"
  are actually output last. That is, the front of the queue contains the "last"
  element according to the weak ordering imposed by Compare.

### Member types

- **`container_type`** — `Container`
- **`value_compare`** — `Compare`
- **`value_type`** — `Container::value_type`
- **`size_type`** — Container::size_type
- **`reference`** — `Container::reference`
- **`const_reference`** — `Container::const_reference`

### Member objects

- **Container c** — the underlying container (protected member object)
- **Compare comp** — the comparison function object (protected member object)

### Member functions

- **(constructor)** — constructs the `priority_queue` (public member function)
- **(destructor)** — destructs the `priority_queue` (public member function)
- **operator=** — assigns values to the container adaptor (public member
  function)

**Element access**

- **top** — accesses the top element (public member function)

**Capacity**

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **size** — returns the number of elements (public member function)

**Modifiers**

- **push** — inserts element and sorts the underlying container (public member
  function)
- **push_range (C++23)** — inserts a range of elements and sorts the underlying
  container (public member function)
- **emplace (C++11)** — constructs element in-place and sorts the underlying
  container (public member function)
- **pop** — removes the top element (public member function)
- **swap (C++11)** — swaps the contents (public member function)

### Non-member functions

- **std::swap(std::priority_queue) (C++11)** — specializes the `std::swap`
  algorithm (function template)

### Helper classes

- **std::uses_allocator<std::priority_queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

### Deduction guides
*(since C++17)*

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges-aware construction
      and insertion for containers

### Example

```cpp
#include <concepts>
#include <functional>
#include <iostream>
#include <queue>
#include <ranges>
#include <string_view>
#include <vector>

template<typename T>
void print(std::string_view name, T const& q)
{
    std::cout << name << ": \t";
    for (auto const& n : q)
        std::cout << n << ' ';
    std::cout << '\n';
}

template<typename Adaptor>
requires (std::ranges::input_range<typename Adaptor::container_type>)
void print(std::string_view name, const Adaptor& adaptor)
{
    struct Printer : Adaptor // to access protected Adaptor::Container c;
    {
        void print(std::string_view name) const { ::print(name, this->c); }
    };

    static_cast<Printer const&>(adaptor).print(name);
}

int main()
{
    const auto data = {1, 8, 5, 6, 3, 4, 0, 9, 7, 2};
    print("data", data);

    std::priority_queue<int> q1; // Max priority queue
    for (int n : data)
        q1.push(n);

    print("q1", q1);

    // Min priority queue
    // std::greater<int> makes the max priority queue act as a min priority queue
    std::priority_queue<int, std::vector<int>, std::greater<int>>
        minq1(data.begin(), data.end());

    print("minq1", minq1);

    // Second way to define a min priority queue
    std::priority_queue minq2(data.begin(), data.end(), std::greater<int>());

    print("minq2", minq2);

    // Using a custom function object to compare elements.
    struct
    {
        bool operator()(const int l, const int r) const { return l > r; }
    } customLess;
    std::priority_queue minq3(data.begin(), data.end(), customLess);

    print("minq3", minq3);

    // Using lambda to compare elements.
    auto cmp = [](int left, int right) { return (left ^ 1) < (right ^ 1); };
    std::priority_queue<int, std::vector<int>, decltype(cmp)> q5(cmp);

    for (int n : data)
        q5.push(n);

    print("q5", q5);
}
```

Output:

```text
data:   1 8 5 6 3 4 0 9 7 2
q1:     9 8 7 6 5 4 3 2 1 0
minq1:  0 1 2 3 4 5 6 7 8 9
minq2:  0 1 2 3 4 5 6 7 8 9
minq3:  0 1 2 3 4 5 6 7 8 9
q5:     8 9 6 7 4 5 2 3 0 1
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 307 | C++98 | `Container` could not be `std::vector<bool>` | allowed
  LWG 2684 | C++98 | `priority_queue` takes a comparator but lacked member
      typedef for it | added

### See also

- **vector** — dynamic contiguous array (class template)
- **vector<bool>** — space-efficient dynamic bitset (class template
  specialization)
- **deque** — double-ended queue (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue*
