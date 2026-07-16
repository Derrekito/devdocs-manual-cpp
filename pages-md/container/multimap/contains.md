# std::multimap<Key,T,Compare,Allocator>::contains

```cpp
bool contains( const Key& key ) const;  // (1) (since C++20)
template< class K > bool contains( const K& x ) const;  // (2) (since C++20)
```

1) Checks if there is an element with key equivalent to `key` in the container.

2) Checks if there is an element with key that compares *equivalent* to the
   value `x`. This overload participates in overload resolution only if the
   qualified-id `Compare::is_transparent` is valid and denotes a type. It allows
   calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the element to search for
- **x** — a value of any type that can be transparently compared with a key

### Return value

`true` if there is such an element, otherwise `false`.

### Complexity

Logarithmic in the size of the container.

### Example

```cpp
#include <iostream>
#include <map>

int main()
{
    std::multimap<int, char> example = {{1, 'a'}, {2, 'b'}};

    for (int x : {2, 5})
    {
        if (example.contains(x))
            std::cout << x << ": Found\n";
        else
            std::cout << x << ": Not found\n";
    }
}
```

Output:

```text
2: Found
5: Not found
```

### See also

- **find** — finds element with specific key (public member function)
- **count** — returns the number of elements matching specific key (public
  member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/contains*
