# std::multimap<Key,T,Compare,Allocator>::key_comp

```cpp
key_compare key_comp() const;
```

Returns the function object that compares the keys, which is a copy of this
container's constructor argument `comp`.

### Parameters

(none)

### Return value

The key comparison function object.

### Complexity

Constant.

### Example

```cpp
#include <cassert>
#include <iostream>
#include <map>

// Example module 97 key compare function
struct ModCmp
{
    bool operator()(const int lhs, const int rhs) const
    {
        return (lhs % 97) < (rhs % 97);
    }
};

int main()
{
    std::multimap<int, char, ModCmp> cont;
    cont = {{1, 'a'}, {2, 'b'}, {3, 'c'}, {4, 'd'}, {5, 'e'}};

    auto comp_func = cont.key_comp();

    for (auto it : cont)
    {
        bool before = comp_func(it.first, 100);
        bool after = comp_func(100, it.first);

        std::cout << '(' << it.first << ',' << it.second;
        if (!before && !after)
            std::cout << ") equivalent to key 100\n";
        else if (before)
            std::cout << ") goes before key 100\n";
        else if (after)
            std::cout << ") goes after key 100\n";
        else
            assert(0); // Cannot happen
    }
}
```

Output:

```text
(1,a) goes before key 100
(2,b) goes before key 100
(3,c) equivalent to key 100
(4,d) goes after key 100
(5,e) goes after key 100
```

### See also

- **value_comp** — returns the function that compares keys in objects of type
  `value_type` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/key_comp*
