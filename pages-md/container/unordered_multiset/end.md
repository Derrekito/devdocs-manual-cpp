# std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::end, std::unordered_multiset<Key,Hash,KeyEqual,Allocator>::cend

```cpp
iterator end() noexcept;  // (1) (since C++11)
const_iterator end() const noexcept;  // (2) (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the
`unordered_multiset`.

This element acts as a placeholder; attempting to access it results in undefined
behavior.

### Parameters

(none)

### Return value

Iterator to the element following the last element.

### Complexity

Constant.

### Notes

Because both `iterator` and `const_iterator` are constant iterators (and may in
fact be the same type), it is not possible to mutate the elements of the
container through an iterator returned by any of these member functions.

### Example

```cpp
#include <iostream>
#include <iterator>
#include <string>
#include <unordered_set>

int main()
{
    const std::unordered_multiset<std::string> words =
    {
        "some", "words", "to", "count",
        "count", "these", "words"
    };

    for (auto it = words.begin(); it != words.end(); )
    {
        auto count = words.count(*it);
        std::cout << *it << ":\t" << count << '\n';
        std::advance(it, count); // all count elements have equivalent keys
    }
}
```

Possible output:

```text
some:   1
words:  2
to:     1
count:  2
these:  1
```

### See also

- **begincbegin** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_multiset/end*
