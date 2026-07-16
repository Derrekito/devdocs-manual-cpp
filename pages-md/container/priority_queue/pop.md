# std::priority_queue<T,Container,Compare>::pop

```cpp
void pop();
```

Removes the top element from the priority queue. Effectively calls
`std::pop_heap(c.begin(), c.end(), comp); c.pop_back();`.

### Parameters

(none)

### Return value

(none)

### Complexity

Logarithmic number of comparisons plus the complexity of `Container::pop_back`.

### Example

```cpp
#include <iostream>
#include <queue>

struct Event
{
    int priority{};
    char data{' '};

    friend bool operator<(Event const& lhs, Event const& rhs)
    {
        return lhs.priority < rhs.priority;
    }

    friend std::ostream& operator<<(std::ostream& os, Event const& e)
    {
        return os << '{' << e.priority << ", '" << e.data << "'}";
    }
};

int main()
{
    std::priority_queue<Event> events;

    std::cout << "Fill the events queue:\t";

    for (auto const e : {Event{6,'L'}, {8,'I'}, {9,'S'}, {1,'T'}, {5,'E'}, {3,'N'}})
    {
        std::cout << e << ' ';
        events.push(e);
    }

    std::cout << "\nProcess events:\t\t";

    for (; !events.empty(); events.pop())
    {
        Event const& e = events.top();
        std::cout << e << ' ';
    }

    std::cout << '\n';
}
```

Output:

```text
Fill the events queue:  {6, 'L'} {8, 'I'} {9, 'S'} {1, 'T'} {5, 'E'} {3, 'N'}
Process events:         {9, 'S'} {8, 'I'} {6, 'L'} {5, 'E'} {3, 'N'} {1, 'T'}
```

### See also

- **emplace (C++11)** — constructs element in-place and sorts the underlying
  container (public member function)
- **push** — inserts element and sorts the underlying container (public member
  function)
- **top** — accesses the top element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/pop*
