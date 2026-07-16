# std::ranges::view_interface

```cpp
template< class D >
    requires std::is_class_v<D> && std::same_as<D, std::remove_cv_t<D>>
class view_interface;  // (since C++20)
```

`std::ranges::view_interface` is a helper class template for defining a view
interface.

`view_interface` is typically used with CRTP:

```cpp
class my_view : public std::ranges::view_interface<my_view>
{
public:
    auto begin() const { /*...*/ }
    auto end() const { /*...*/ }
    // empty() is provided if begin() returns a forward iterator
    // and end() returns a sentinel for it.
};
```

### Member functions

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function)
- **data (C++20)** — gets the address of derived view's data. Provided if its
  iterator type satisfies `contiguous_iterator`. (public member function)
- **size (C++20)** — returns the number of elements in the derived view.
  Provided if it satisfies `forward_range` and its sentinel and iterator type
  satisfy `sized_sentinel_for`. (public member function)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member
  function)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function)

### Example

```cpp
#include <iostream>
#include <ranges>
#include <vector>

template<class T, class A>
class VectorView : public std::ranges::view_interface<VectorView<T, A>>
{
public:
    VectorView() = default;

    VectorView(const std::vector<T, A>& vec) :
        m_begin(vec.cbegin()), m_end(vec.cend())
    {}

    auto begin() const { return m_begin; }

    auto end() const { return m_end; }

private:
    typename std::vector<T, A>::const_iterator m_begin{}, m_end{};
};

int main()
{
    std::vector<int> v = {1, 4, 9, 16};

    VectorView view_over_v{v};

    // We can iterate with begin() and end().
    for (int n : view_over_v)
        std::cout << n << ' ';
    std::cout << '\n';

    // We get operator[] for free when inheriting from view_interface
    // since we satisfy the random_access_range concept.
    for (std::ptrdiff_t i = 0; i != view_over_v.size(); ++i)
        std::cout << "v[" << i << "] = " << view_over_v[i] << '\n';
}
```

Output:

```text
1 4 9 16
v[0] = 1
v[1] = 4
v[2] = 9
v[3] = 16
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3549 | C++20 | `view_interface` was required to be derived from
      `view_base`, which sometimes required multiple `view_base` subobjects in a
      view | inheritance removed

### See also

- **ranges::subrange (C++20)** — combines an iterator-sentinel pair into a
  `view` (class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/view_interface*
