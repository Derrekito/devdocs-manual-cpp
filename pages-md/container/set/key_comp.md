# std::set<Key,Compare,Allocator>::key_comp

```cpp
key_compare key_comp() const;
```

Returns the function object that compares the keys, which is a copy of this
container's constructor argument `comp`. It is the same as `value_comp`.

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
#include <set>

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
    std::set<int, ModCmp> cont{1, 2, 3, 4, 5};

    auto comp_func = cont.key_comp();

    for (int key : cont)
    {
        bool before = comp_func(key, 100);
        bool after = comp_func(100, key);
        if (!before && !after)
            std::cout << key << " equivalent to key 100\n";
        else if (before)
            std::cout << key << " goes before key 100\n";
        else if (after)
            std::cout << key << " goes after key 100\n";
        else
            assert(0); // Cannot happen
    }
}
```

Output:

```text
1 goes before key 100
2 goes before key 100
3 equivalent to key 100
4 goes after key 100
5 goes after key 100
```

### See also

- **value_comp** — returns the function that compares keys in objects of type
  `value_type` (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/key_comp*
