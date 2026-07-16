# std::unordered_set<Key,Hash,KeyEqual,Allocator>::max_bucket_count

```cpp
size_type max_bucket_count() const;  // (since C++11)
```

Returns the maximum number of buckets the container is able to hold due to
system or library implementation limitations.

### Parameters

(none)

### Return value

Maximum number of buckets.

### Complexity

Constant.

### Example

```cpp
#include <iostream>
#include <unordered_set>

int main()
{
    struct Ha { std::size_t operator()(long x) const { return std::hash<long>{}(x); }; };

    auto c1 = std::unordered_set<char>{};
    auto c2 = std::unordered_set<long>{};
    auto c3 = std::unordered_set<long, std::hash<int>>{};
    auto c4 = std::unordered_set<long, Ha>{};

    std::cout
        << "Max bucket count of\n" << std::hex << std::showbase
        << "c1: " << c1.max_bucket_count() << '\n'
        << "c2: " << c2.max_bucket_count() << '\n'
        << "c3: " << c3.max_bucket_count() << '\n'
        << "c4: " << c4.max_bucket_count() << '\n'
        ;
}
```

Possible output:

```text
Max bucket count of
c1: 0xfffffffffffffff
c2: 0xfffffffffffffff
c3: 0xfffffffffffffff
c4: 0xaaaaaaaaaaaaaaa
```

### See also

- **bucket_count** — returns the number of buckets (public member function)

---
*Source: https://en.cppreference.com/w/cpp/container/unordered_set/max_bucket_count*
