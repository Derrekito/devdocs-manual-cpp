# std::set<Key,Compare,Allocator>::count

```cpp
size_type count( const Key& key ) const;  // (1)
template< class K >
size_type count( const K& x ) const;  // (2) (since C++14)
```

Returns the number of elements with key that compares *equivalent* to the
specified argument.

1) Returns the number of elements with key `key`. This is either 1 or 0 since
   this container does not allow duplicates.

2) Returns the number of elements with key that compares equivalent to the value
   `x`. This overload participates in overload resolution only if the
   qualified-id `Compare::is_transparent` is valid and denotes a type. It allows
   calling this function without constructing an instance of `Key`.

### Parameters

- **key** — key value of the elements to count
- **x** — alternative value to compare to the keys

### Return value

Number of elements with key that compares *equivalent* to `key` or `x`, which,
for overload (1), is either 1 or 0.

### Complexity

Logarithmic in the size of the container plus linear in the number of elements
found.

### Example

```cpp
#include <iostream>
#include <set>

struct S
{
    int x;
    S(int i) : x{i} { std::cout << "S{" << i << "} "; }
    bool operator<(S const& s) const { return x < s.x; }
};

struct R
{
    int x;
    R(int i) : x{i} { std::cout << "R{" << i << "} "; }
    bool operator<(R const& r) const { return x < r.x; }
};

bool operator<(R const& r, int i) { return r.x < i; }
bool operator<(int i, R const& r) { return i < r.x; }

int main()
{
    std::set<int> t{3, 1, 4, 1, 5};
    std::cout << t.count(1) << ", " << t.count(2) << ".\n";

    std::set<S> s{3, 1, 4, 1, 5};
    std::cout << ": " << s.count(1) << ", " << s.count(2) << ".\n";
        // Two temporary objects S{1} and S{2} were created.
        // Comparison function object is defaulted std::less<S>,
        // which is not transparent (has no is_transparent member type).

    std::set<R, std::less<>> r{3, 1, 4, 1, 5};
    std::cout << ": " << r.count(1) << ", " << r.count(2) << ".\n";
        // C++14 heterogeneous lookup; temporary objects were not created.
        // Comparator std::less<void> has predefined is_transparent.
}
```

Output:

```text
1, 0.
S{3} S{1} S{4} S{1} S{5} : S{1} 1, S{2} 0.
R{3} R{1} R{4} R{1} R{5} : 1, 0.
```

### See also

- **find** — finds element with specific key (public member function)
- **equal_range** — returns range of elements matching a specific key (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/container/set/count*
