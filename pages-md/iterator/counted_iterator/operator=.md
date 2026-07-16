# std::counted_iterator<I>::operator=

```cpp
template< class I2 >
    requires std::assignable_from<I&, const I2&>
constexpr counted_iterator& operator=( const counted_iterator<I2>& other );  // (since C++20)
```

The underlying iterator and *length* are assigned with those of `other`.

### Parameters

- **other** — iterator adaptor to assign from

### Return value

`*this`

### Example

```cpp
#include <algorithm>
#include <cassert>
#include <initializer_list>
#include <iterator>

int main()
{
    auto a = {3, 1, 4, 1, 5, 9, 2};
    std::counted_iterator<std::initializer_list<int>::iterator> p(begin(a), size(a) - 2);
    std::counted_iterator<std::initializer_list<int>::iterator> q;
    assert(q.count() == 0);
    assert(q.count() != p.count());
    q = p;
    assert(q.count() == p.count());
    assert(std::ranges::equal(p, std::default_sentinel, q, std::default_sentinel));
}
```

### See also

- **(constructor) (C++20)** — constructs a new iterator adaptor (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/counted_iterator/operator%3D*
