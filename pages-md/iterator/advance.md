# std::advance

```cpp
template< class InputIt, class Distance >
void advance( InputIt& it, Distance n );  // (until C++17)
template< class InputIt, class Distance >
constexpr void advance( InputIt& it, Distance n );  // (since C++17)
```

Increments given iterator `it` by `n` elements.

If `n` is negative, the iterator is decremented. In this case, `InputIt` must
meet the requirements of LegacyBidirectionalIterator, otherwise the behavior is
undefined.

### Parameters

- **it** — iterator to be advanced
- **n** — number of elements `it` should be advanced

**Type requirements**

**-`InputIt` must meet the requirements of LegacyInputIterator.**

### Return value

(none)

### Complexity

Linear.

However, if `InputIt` additionally meets the requirements of
LegacyRandomAccessIterator, complexity is constant.

### Notes

The behavior is undefined if the specified sequence of increments or decrements
would require that a non-incrementable iterator (such as the past-the-end
iterator) is incremented, or that a non-decrementable iterator (such as the
front iterator or the singular iterator) is decremented.

### Possible implementation

See also the implementations in libstdc++ and libc++.

```cpp
namespace detail
{
    template<class It>
    void do_advance(It& it, typename std::iterator_traits<It>::difference_type n,
                    std::input_iterator_tag)
    {
        while (n > 0)
        {
            --n;
            ++it;
        }
    }

    template<class It>
    void do_advance(It& it, typename std::iterator_traits<It>::difference_type n,
                    std::bidirectional_iterator_tag)
    {
        while (n > 0)
        {
            --n;
            ++it;
        }
        while (n < 0)
        {
            ++n;
            --it;
        }
    }

    template<class It>
    void do_advance(It& it, typename std::iterator_traits<It>::difference_type n,
                    std::random_access_iterator_tag)
    {
        it += n;
    }
} // namespace detail

template<class It, class Distance>
void advance(It& it, Distance n)
{
    detail::do_advance(it, typename std::iterator_traits<It>::difference_type(n),
                       typename std::iterator_traits<It>::iterator_category());
}
```

```cpp
template<class It, class Distance>
constexpr void advance(It& it, Distance n)
{
    using category = typename std::iterator_traits<It>::iterator_category;
    static_assert(std::is_base_of_v<std::input_iterator_tag, category>);

    auto dist = typename std::iterator_traits<It>::difference_type(n);
    if constexpr (std::is_base_of_v<std::random_access_iterator_tag, category>)
        it += dist;
    else
    {
        while (dist > 0)
        {
            --dist;
            ++it;
        }
        if constexpr (std::is_base_of_v<std::bidirectional_iterator_tag, category>)
            while (dist < 0)
            {
                ++dist;
                --it;
            }
    }
}
```

### Example

```cpp
#include <iostream>
#include <iterator>
#include <vector>

int main()
{
    std::vector<int> v{3, 1, 4};

    auto vi = v.begin();
    std::advance(vi, 2);
    std::cout << *vi << ' ';

    vi = v.end();
    std::advance(vi, -2);
    std::cout << *vi << '\n';
}
```

Output:

```text
4 1
```

### See also

- **next (C++11)** — increment an iterator (function template)
- **prev (C++11)** — decrement an iterator (function template)
- **distance** — returns the distance between two iterators (function template)
- **ranges::advance (C++20)** — advances an iterator by given distance or to a
  given bound (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/iterator/advance*
