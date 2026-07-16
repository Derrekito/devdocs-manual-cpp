# std::unordered_set<Key,Hash,KeyEqual,Allocator>::contains

```cpp
bool contains( const Key& key ) const;  // (1) (since C++20)
template< class K > bool contains( const K& x ) const;  // (2) (since C++20)
```

1) Checks if there is an element with key equivalent to `key` in the container.

2) Checks if there is an element with key that compares *equivalent* to the
   value `x`. This overload participates in overload resolution only if
   `Hash::is_transparent` and `KeyEqual::is_transparent` are valid and each
   denotes a type. This assumes that such `Hash` is callable with both `K` and
   `Key` type, and that the `KeyEqual` is transparent, which, together, allows
   calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the element to search for
- **x** — a value of any type that can be transparently compared with a key

### Return value

`true` if there is such an element, otherwise `false`.

### Complexity

Constant on average, worst case linear in the size of the container.

### Example

```cpp
#include <iostream>
#include <unordered_set>

int main()
{
    std::unordered_set<int> example = {1, 2, 3, 4};

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
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/contains*
