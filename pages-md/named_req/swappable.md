# C++ named requirements: Swappable

Any lvalue or rvalue of this type can be swapped with any lvalue or rvalue of
some other type, using unqualified function call `swap()` in the context where
both `std::swap` and the user-defined `swap()`s are visible.

### Requirements

Type U is swappable with type T if, for any object u of type U and any object t
of type T,

  Expression | Requirements | Semantics
  `#include <algorithm> // until C++11 #include <utility> // since C++11 using
      std::swap; swap(u, t);` | After the call, the value of `t` is the value
      held by `u` before the call, and the value of `u` is the value held by `t`
      before the call. | Calls the function named `swap()` found by overload
      resolution among all functions with that name that are found by
      argument-dependent lookup and the two `std::swap` templates defined in the
      header `<algorithm>`(until C++11)`<utility>`(since C++11).
  `#include <algorithm> // until C++11 #include <utility> // since C++11 using
      std::swap; swap(t, u);` | Same | Same

Many standard library functions (for example, many algorithms) expect their
arguments to satisfy Swappable, which means that any time the standard library
performs a swap, it uses the equivalent of `using std::swap; swap(t, u);`.

Typical implementations either

1) Define a non-member swap in the enclosing namespace, which may forward to a
   member swap if access to non-public data members is required.

2) Define a friend function in-class (this approach hides the class-specific
   swap from name lookup other than ADL).

### Notes

It is unspecified whether `<algorithm>`(until C++11)`<utility>`(since C++11) is
actually included when the standard library functions perform the swap, so the
user-provided `swap()` should not expect it to be included.

### Example

```cpp
#include <iostream>
#include <vector>

struct IntVector
{
    std::vector<int> v;

    IntVector& operator=(IntVector) = delete; // not assignable

    void swap(IntVector& other)
    {
        v.swap(other.v);
    }

    void operator()(auto rem, auto term = " ")
    {
        std::cout << rem << "{{";
        for (int n{}; int e : v)
            std::cout << (n++ ? ", " : "") << e;
        std::cout << "}}" << term;
    }
};

void swap(IntVector& v1, IntVector& v2)
{
    v1.swap(v2);
}

int main()
{
    IntVector v1{{1, 1, 1, 1}}, v2{{2222, 2222}};

    auto prn = [&]{ v1("v1", ", "), v2("v2", ";\n"); };

//  std::swap(v1, v2); // Compiler error! std::swap requires MoveAssignable
    prn();
    std::iter_swap(&v1, &v2); // OK: library calls unqualified swap()
    prn();
    std::ranges::swap(v1, v2); // OK: library calls unqualified swap()
    prn();
}
```

Output:

```text
v1{{1, 1, 1, 1}}, v2{{2222, 2222}};
v1{{2222, 2222}}, v2{{1, 1, 1, 1}};
v1{{1, 1, 1, 1}}, v2{{2222, 2222}};
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 226 | C++98 | it was unclear how the standard library uses `swap` |
      clarified to use both `std::` and ADL-found `swap`

### See also

- **is_swappable_withis_swappableis_nothrow_swappable_withis_nothrow_swappable
  (C++17)(C++17)(C++17)(C++17)** — checks if objects of a type can be swapped
  with objects of same or different type (class template)
- **swappableswappable_with (C++20)** — specifies that a type can be swapped or
  that two types can be swapped with each other (concept)

---
*Source: https://en.cppreference.com/w/cpp/named_req/Swappable*
