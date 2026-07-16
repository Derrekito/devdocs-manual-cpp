# std::multiset<Key,Compare,Allocator>::end, std::multiset<Key,Compare,Allocator>::cend

```cpp
iterator end();  // (until C++11)
iterator end() noexcept;  // (since C++11)
const_iterator end() const;  // (until C++11)
const_iterator end() const noexcept;  // (since C++11)
const_iterator cend() const noexcept;  // (3) (since C++11)
```

Returns an iterator to the element following the last element of the `multiset`.

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
#include <set>
#include <string>

int main()
{
    const std::multiset<std::string> words =
    {
        "some", "not", "sorted", "words",
        "will", "come", "out", "sorted",
    };

    for (auto it = words.begin(); it != words.end(); )
    {
        auto count = words.count(*it);
        std::cout << *it << ":\t" << count << '\n';
        std::advance(it, count); // all count elements have equivalent keys
    }
}
```

Output:

```text
come:        1
not:        1
out:        1
some:        1
sorted:        2
will:        1
words:        1
```

### See also

- **begincbegin (C++11)** — returns an iterator to the beginning (public member
  function)
- **endcend (C++11)(C++14)** — returns an iterator to the end of a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/multiset/end*
