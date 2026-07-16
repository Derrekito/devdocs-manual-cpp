# std::multimap<Key,T,Compare,Allocator>::end, std::multimap<Key,T,Compare,Allocator>::cend

```cpp
iterator end();  // (until C++11)
iterator end() noexcept;  // (since C++11)
const_iterator end() const;  // (until C++11)
const_iterator end() const noexcept;  // (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the `multimap`.

This element acts as a placeholder; attempting to access it results in undefined
behavior.

### Parameters

(none)

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <cstddef>
#include <iostream>
#include <map>
#include <string>

int main()
{
    auto show_node = [](const auto& node, char ending = '\n')
    {
        std::cout << "{ " << node.first << ", " << node.second << " }" << ending;
    };

    std::multimap<std::size_t, std::string> mmap;
    assert(mmap.begin() == mmap.end());   // OK
    assert(mmap.cbegin() == mmap.cend()); // OK

    mmap.insert({ sizeof(long), "LONG" });
    show_node(*(mmap.cbegin()));
    assert(mmap.begin() != mmap.end());   // OK
    assert(mmap.cbegin() != mmap.cend()); // OK
    mmap.begin()->second = "long";
    show_node(*(mmap.cbegin()));

    mmap.insert({ sizeof(int), "int" });
    show_node(*mmap.cbegin());

    mmap.insert({ sizeof(short), "short" });
    show_node(*mmap.cbegin());

    mmap.insert({ sizeof(char), "char" });
    show_node(*mmap.cbegin());

    mmap.insert({{ sizeof(float), "float" }, { sizeof(double), "double"}});

    std::cout << "mmap = { ";
    std::for_each(mmap.cbegin(), mmap.cend(), [&](const auto& n) { show_node(n, ' '); });
    std::cout << "};\n";
}
```

Possible output:

```text
{ 8, LONG }
{ 8, long }
{ 4, int }
{ 2, short }
{ 1, char }
mmap = { { 1, char } { 2, short } { 4, int } { 4, float } { 8, long } { 8, double } };
```

### See also

- **begincbegin (C++11)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/end*
