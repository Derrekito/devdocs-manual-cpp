# std::unordered_map<Key,T,Hash,KeyEqual,Allocator>::count

```cpp
size_type count( const Key& key ) const;  // (1) (since C++11)
template< class K >
size_type count( const K& x ) const;  // (2) (since C++20)
```

1) Returns the number of elements with key that compares equal to the specified
   argument `key`, which is either `​0​` or `1` since this container does not
   allow duplicates.

2) Returns the number of elements with key that compares equivalent to the
   specified argument `x`. This overload participates in overload resolution
   only if `Hash::is_transparent` and `KeyEqual::is_transparent` are valid and
   each denotes a type. This assumes that such `Hash` is callable with both `K`
   and `Key` type, and that the `KeyEqual` is transparent, which, together,
   allows calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the elements to count
- **x** — a value of any type that can be transparently compared with a key

### Return value

1) Number of elements with key `key`, that is either `1` or `​0​`.

2) Number of elements with key that compares equivalent to `x`.

### Complexity

Constant on average, worst case linear in the size of the container.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generic_unordered_lookup` | 201811L | (C++20) | Heterogeneous
      comparison lookup in unordered associative containers, overload (2)

### Example

```cpp
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<int, std::string> dict = {
        {1, "one"}, {6, "six"}, {3, "three"}
    };
    dict.insert({4, "four"});
    dict.insert({5, "five"});
    dict.insert({6, "six"});

    std::cout << "dict: { ";
    for (auto const& [key, value] : dict)
        std::cout << '[' << key << "]=" << value << ' ';
    std::cout << "}\n\n";

    for (int i{1}; i != 8; ++i)
        std::cout << "dict.count(" << i << ") = " << dict.count(i) << '\n';
}
```

Possible output:

```text
dict: { [5]=five [4]=four [1]=one [6]=six [3]=three }

dict.count(1) = 1
dict.count(2) = 0
dict.count(3) = 1
dict.count(4) = 1
dict.count(5) = 1
dict.count(6) = 1
dict.count(7) = 0
```

### See also

- **find** — finds element with specific key (public member function)
- **contains (C++20)** — checks if the container contains element with specific
  key (public member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_map/count*
