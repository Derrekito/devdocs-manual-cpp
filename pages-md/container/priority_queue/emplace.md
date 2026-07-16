# std::priority_queue<T,Container,Compare>::emplace

```cpp
template< class... Args >
void emplace( Args&&... args );  // (since C++11)
```

Pushes a new element to the priority queue. The element is constructed in-place,
i.e. no copy or move operations are performed. The constructor of the element is
called with exactly the same arguments as supplied to the function. Effectively
calls

```cpp
c.emplace_back(std::forward<Args>(args)...);
std::push_heap(c.begin(), c.end(), comp);
```

### Parameters

- **args** — arguments to forward to the constructor of the element

### Return value

(none)

### Complexity

Logarithmic number of comparisons plus the complexity of
`Container::emplace_back`.

### Example

```cpp
#include <iostream>
#include <queue>

struct S
{
    int id;

    S(int i, double d, std::string s) : id{i}
    {
        std::cout << "S::S(" << i << ", " << d << ", \"" << s << "\");\n";
    }
    friend bool operator< (S const& x, S const& y) { return x.id < y.id; }
};

int main()
{
    std::priority_queue<S> queue;
    queue.emplace(42, 3.14, "C++");
    std::cout << "id: " << queue.top().id << '\n';
}
```

Output:

```text
S::S(42, 3.14, "C++")
id = 42
```

### See also

- **push** — inserts element and sorts the underlying container (public member
  function)
- **pop** — removes the top element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/emplace*
