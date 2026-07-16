# std::stack<T,Container>::top

```cpp
reference top();
const_reference top() const;
```

Returns reference to the top element in the stack. This is the most recently
pushed element. This element will be removed on a call to `pop()`. Effectively
calls `c.back()`.

### Parameters

(none)

### Return value

Reference to the last element.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <stack>

void reportStackSize(const std::stack<int>& s)
{
    std::cout << s.size() << " elements on stack\n";
}

void reportStackTop(const std::stack<int>& s)
{
    // Leaves element on stack
    std::cout << "Top element: " << s.top() << '\n';
}

int main()
{
    std::stack<int> s;
    s.push(2);
    s.push(6);
    s.push(51);

    reportStackSize(s);
    reportStackTop(s);

    reportStackSize(s);
    s.pop();

    reportStackSize(s);
    reportStackTop(s);
}
```

Output:

```text
3 elements on stack
Top element: 51
3 elements on stack
2 elements on stack
Top element: 6
```

### See also

- **push** — inserts element at the top (public member function)
- **pop** — removes the top element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/stack/top*
