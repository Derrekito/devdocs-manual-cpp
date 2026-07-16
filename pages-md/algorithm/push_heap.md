# std::push_heap

```cpp
template< class RandomIt >
void push_heap( RandomIt first, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void push_heap( RandomIt first, RandomIt last );  // (since C++20)
template< class RandomIt, class Compare >
void push_heap( RandomIt first, RandomIt last, Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void push_heap( RandomIt first, RandomIt last, Compare comp );  // (since C++20)
```

Inserts the element at the position `last - 1` into the heap `[``first``,``last
- 1``)`. The heap after the insertion will be `[``first``,``last``)`.

1) The heap is with respect to `operator<`.

2) The heap is with respect to `comp`.

If `[``first``,``last - 1``)` is not a heap, the behavior is undefined.

### Parameters

- **first, last** — the range indicating the heap after the insertion
- **comp** — comparison function object (i.e. an object that satisfies the
  requirements of Compare) which returns `true` if the first argument is *less*
  than the second. The signature of the comparison function should be equivalent
  to the following: `bool cmp(const Type1& a, const Type2& b);` While the
  signature does not need to have `const&`, the function must not modify the
  objects passed to it and must be able to accept all values of type (possibly
  const) `Type1` and `Type2` regardless of value category (thus, `Type1&` is not
  allowed, nor is `Type1` unless for `Type1` a move is equivalent to a
  copy(since C++11)). The types Type1 and Type2 must be such that an object of
  type RandomIt can be dereferenced and then implicitly converted to both of
  them.

**Type requirements**

**-`RandomIt` must meet the requirements of LegacyRandomAccessIterator.**

**-The type of dereferenced `RandomIt` must meet the requirements of MoveAssignable and MoveConstructible.**

**-`Compare` must meet the requirements of Compare.**

### Return value

(none)

### Complexity

Given \(\scriptsize N\)N as `std::distance(first, last)`:

1) At most \(\scriptsize log(N)\)log(N) comparisons using `operator<`.

2) At most \(\scriptsize log(N)\)log(N) applications of the comparison function
   `comp`.

### Notes

A *heap with respect to `comp`* (max heap) is a random access range
`[``first``,``last``)` that has the following properties:

- Given \(\scriptsize N\)N as `last - first`, for all integer `i` where
  \(\scriptsize 0 < i < N\)0 < i < N, `bool(comp(first[(i - 1) / 2], first[i]))`
  is `false`.
- A new element can be added using `std::push_heap`, in \(\scriptsize
  \mathcal{O}(\log N)\)𝓞(log N) time.
- `*first` can be removed using `std::pop_heap`, in \(\scriptsize
  \mathcal{O}(\log N)\)𝓞(log N) time.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <string_view>
#include <vector>

void println(std::string_view rem, std::vector<int> const& v)
{
    std::cout << rem;
    for (int e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    std::vector<int> v{3, 1, 4, 1, 5, 9};

    std::make_heap(v.begin(), v.end());
    println("after make_heap: ", v);

    v.push_back(6);
    println("after push_back: ", v);

    std::push_heap(v.begin(), v.end());
    println("after push_heap: ", v);
}
```

Output:

```text
after make_heap: 9 5 4 1 1 3
after push_back: 9 5 4 1 1 3 6
after push_heap: 9 5 6 1 1 3 4
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 193 | C++98 | heap required `*first` to be the largest element | there can
      be elements equal to `*first`
  LWG 2166 | C++98 | the heap requirement did not match the definition of max
      heap closely enough | requirement improved

### See also

- **is_heap (C++11)** — checks if the given range is a max heap (function
  template)
- **is_heap_until (C++11)** — finds the largest subrange that is a max heap
  (function template)
- **make_heap** — creates a max heap out of a range of elements (function
  template)
- **pop_heap** — removes the largest element from a max heap (function template)
- **sort_heap** — turns a max heap into a range of elements sorted in ascending
  order (function template)
- **ranges::push_heap (C++20)** — adds an element to a max heap (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/push_heap*
