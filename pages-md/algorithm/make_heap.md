# std::make_heap

```cpp
template< class RandomIt >
void make_heap( RandomIt first, RandomIt last );  // (until C++20)
template< class RandomIt >
constexpr void make_heap( RandomIt first, RandomIt last );  // (since C++20)
template< class RandomIt, class Compare >
void make_heap( RandomIt first, RandomIt last, Compare comp );  // (until C++20)
template< class RandomIt, class Compare >
constexpr void make_heap( RandomIt first, RandomIt last, Compare comp );  // (since C++20)
```

Constructs a heap in the range `[``first``,``last``)`.

1) The constructed heap is with respect to `operator<`.

2) The constructed heap is with respect to `comp`.

### Parameters

- **first, last** — the range to make the heap from
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

1) At most \(\scriptsize 3N\)3N comparisons using `operator<`.

2) At most \(\scriptsize 3N\)3N applications of the comparison function `comp`.

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
#include <functional>
#include <iostream>
#include <string_view>
#include <vector>

void print(std::string_view text, std::vector<int> const& v = {})
{
    std::cout << text << ": ";
    for (const auto& e : v)
        std::cout << e << ' ';
    std::cout << '\n';
}

int main()
{
    print("Max heap");

    std::vector<int> v{3, 2, 4, 1, 5, 9};
    print("initially, v", v);

    std::make_heap(v.begin(), v.end());
    print("after make_heap, v", v);

    std::pop_heap(v.begin(), v.end());
    print("after pop_heap, v", v);

    auto top = v.back();
    v.pop_back();
    print("former top element", {top});
    print("after removing the former top element, v", v);

    print("\nMin heap");

    std::vector<int> v1{3, 2, 4, 1, 5, 9};
    print("initially, v1", v1);

    std::make_heap(v1.begin(), v1.end(), std::greater<>{});
    print("after make_heap, v1", v1);

    std::pop_heap(v1.begin(), v1.end(), std::greater<>{});
    print("after pop_heap, v1", v1);

    auto top1 = v1.back();
    v1.pop_back();
    print("former top element", {top1});
    print("after removing the former top element, v1", v1);
}
```

Output:

```text
Max heap:
initially, v: 3 2 4 1 5 9
after make_heap, v: 9 5 4 1 2 3
after pop_heap, v: 5 3 4 1 2 9
former top element: 9
after removing the former top element, v: 5 3 4 1 2

Min heap:
initially, v1: 3 2 4 1 5 9
after make_heap, v1: 1 2 4 3 5 9
after pop_heap, v1: 2 3 4 9 5 1
former top element: 1
after removing the former top element, v1: 2 3 4 9 5
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
- **push_heap** — adds an element to a max heap (function template)
- **pop_heap** — removes the largest element from a max heap (function template)
- **sort_heap** — turns a max heap into a range of elements sorted in ascending
  order (function template)
- **priority_queue** — adapts a container to provide priority queue (class
  template)
- **ranges::make_heap (C++20)** — creates a max heap out of a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/make_heap*
