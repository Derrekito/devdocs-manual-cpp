# C++ named requirements: ValueSwappable (since C++11)

Two objects of this type can be dereferenced and the resulting values can be
swapped using unqualified function call `swap()` in the context where both
`std::swap` and the user-defined `swap()`s are visible.

### Requirements

A type T is ValueSwappable if

1. `T` satisfies the LegacyIterator requirements.
1. For any dereferenceable object `x` of type `T` (that is, any value other than
   the end iterator), `*x` satisfies the Swappable requirements.

Many standard library functions expect their arguments to satisfy
ValueSwappable, which means that any time the standard library performs a swap,
it uses the equivalent of `using std::swap; swap(*iter1, *iter2);`.

### Example

```cpp
#include <iostream>
#include <vector>

class IntVector
{
    std::vector<int> v;
//  IntVector& operator=(IntVector); // not assignable (C++98 way)
public:
    IntVector& operator=(IntVector) = delete; // not assignable
    void swap(IntVector& other)
    {
        v.swap(other.v);
    }
};

void swap(IntVector& v1, IntVector& v2)
{
    v1.swap(v2);
}

int main()
{
    IntVector v1, v2;    // IntVector is Swappable, but not MoveAssignable
    IntVector* p1 = &v1;
    IntVector* p2 = &v2; // IntVector* is ValueSwappable
    std::iter_swap(p1, p2); // OK: iter_swap requires ValueSwappable
//  std::swap(v1, v2); // compiler error! std::swap requires MoveAssignable
}
```

### See also

- **indirectly_swappable (C++20)** — specifies that the values referenced by two
  `indirectly_readable` types can be swapped (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/ValueSwappable*
