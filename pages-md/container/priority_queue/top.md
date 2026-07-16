# std::priority_queue<T,Container,Compare>::top

```cpp
const_reference top() const;
```

Returns reference to the top element in the priority queue. This element will be
removed on a call to `pop()`. If default comparison function is used, the
returned element is also the greatest among the elements in the queue.

### Parameters

(none)

### Return value

Reference to the top element as if obtained by a call to `c.front()`.

### Complexity

Constant.

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

- **pop** — removes the top element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/priority_queue/top*
