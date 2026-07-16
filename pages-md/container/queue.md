# std::queue

```cpp
template<
    class T,
    class Container = std::deque<T>
> class queue;
```

The `std::queue` class is a container adaptor that gives the functionality of a
queue - specifically, a FIFO (first-in, first-out) data structure.

The class template acts as a wrapper to the underlying container - only a
specific set of functions is provided. The queue pushes the elements on the back
of the underlying container and pops them from the front.

### Template parameters

- **T** — The type of the stored elements. The behavior is undefined if `T` is
  not the same type as `Container::value_type`.
- **Container** — The type of the underlying container to use to store the
  elements. The container must satisfy the requirements of SequenceContainer.
  Additionally, it must provide the following functions with the usual
  semantics: `back()` `front()` `push_back()` `pop_front()` The standard
  containers `std::deque` and `std::list` satisfy these requirements.

### Member types

- **`container_type`** — `Container`
- **`value_type`** — `Container::value_type`
- **`size_type`** — Container::size_type
- **`reference`** — `Container::reference`
- **`const_reference`** — `Container::const_reference`

### Member objects

- **Container c** — the underlying container (protected member object)

### Member functions

- **(constructor)** — constructs the `queue` (public member function)
- **(destructor)** — destructs the `queue` (public member function)
- **operator=** — assigns values to the container adaptor (public member
  function)

**Element access**

- **front** — access the first element (public member function)
- **back** — access the last element (public member function)

**Capacity**

- **empty** — checks whether the container adaptor is empty (public member
  function)
- **size** — returns the number of elements (public member function)

**Modifiers**

- **push** — inserts element at the end (public member function)
- **push_range (C++23)** — inserts a range of elements at the end (public member
  function)
- **emplace (C++11)** — constructs element in-place at the end (public member
  function)
- **pop** — removes the first element (public member function)
- **swap (C++11)** — swaps the contents (public member function)

### Non-member functions

- **operator==operator!=operator<operator<=operator>operator>=operator<=>
  (C++20)** — lexicographically compares the values of two `queues` (function
  template)
- **std::swap(std::queue) (C++11)** — specializes the `std::swap` algorithm
  (function template)

### Helper classes

- **std::uses_allocator<std::queue> (C++11)** — specializes the
  `std::uses_allocator` type trait (class template specialization)

### Deduction guides
*(since C++17)*

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_containers_ranges` | 202202L | (C++23) | Ranges construction and
      insertion for containers

### Example

```cpp
#include <cassert>
#include <iostream>
#include <queue>

int main()
{
    std::queue<int> q;

    q.push(0); // back pushes 0
    q.push(1); // q = 0 1
    q.push(2); // q = 0 1 2
    q.push(3); // q = 0 1 2 3

    assert(q.front() == 0);
    assert(q.back() == 3);
    assert(q.size() == 4);

    q.pop(); // removes the front element, 0
    assert(q.size() == 3);

    // Print and remove all elements. Note that std::queue does not
    // support begin()/end(), so a range-for-loop cannot be used.
    std::cout << "q: ";
    for (; !q.empty(); q.pop())
        std::cout << q.front() << ' ';
    std::cout << '\n';
    assert(q.size() == 0);
}
```

Output:

```text
q: 1 2 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 307 | C++98 | `std::queue` did not support containers using proxy
      reference types[1] in place of (`const`) `value_type&` | supported

1. Such as containers similar to `std::vector<bool>` with additional support of
   `pop_front()`. The resolution of this DR added support of `std::vector<bool>`
   for `std::stack` and `std::priority_queue`. The changes involving
   `std::queue` are for maintaining consistency.

### See also

- **deque** — double-ended queue (class template)
- **list** — doubly-linked list (class template)

---
*Source: https://en.cppreference.com/w/cpp/container/queue*
