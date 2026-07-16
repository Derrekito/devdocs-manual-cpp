# std::erase_if (std::map)

```cpp
template< class Key, class T, class Compare, class Alloc, class Pred >
typename std::map<Key, T, Compare, Alloc>::size_type
    erase_if( std::map<Key, T, Compare, Alloc>& c, Pred pred );  // (since C++20)
```

Erases all elements that satisfy the predicate `pred` from the container.
Equivalent to

```cpp
auto old_size = c.size();
for (auto first = c.begin(), last = c.end(); first != last;)
{
    if (pred(*first))
        first = c.erase(first);
    else
        ++first;
}
return old_size - c.size();
```

### Parameters

- **c** — container from which to erase
- **pred** — predicate that returns `true` if the element should be erased

### Return value

The number of erased elements.

### Complexity

Linear.

### Example

```cpp
#include <iostream>
#include <map>

void print(auto rem, auto const& container)
{
    std::cout << rem << '{';
    for (char sep[]{0, ' ', 0}; const auto& [key, value] : container)
        std::cout << sep << '{' << key << ", " << value << '}', *sep = ',';
    std::cout << "}\n";
}

int main()
{
    std::map<int, char> data
    {
        {1, 'a'}, {2, 'b'}, {3, 'c'}, {4, 'd'},
        {5, 'e'}, {4, 'f'}, {5, 'g'}, {5, 'g'},
    };
    print("Original:\n", data);

    const auto count = std::erase_if(data, [](const auto& item)
    {
        auto const& [key, value] = item;
        return (key & 1) == 1;
    });

    print("Erase items with odd keys:\n", data);
    std::cout << count << " items removed.\n";
}
```

Output:

```text
Original:
{{1, a}, {2, b}, {3, c}, {4, d}, {5, e}}
Erase items with odd keys:
{{2, b}, {4, d}}
3 items removed.
```

### See also

- **removeremove_if** — removes elements satisfying specific criteria (function
  template)

---
*Source: https://en.cppreference.com/w/cpp/container/map/erase_if*
