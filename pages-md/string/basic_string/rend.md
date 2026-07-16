# std::basic_string<CharT,Traits,Allocator>::rend, std::basic_string<CharT,Traits,Allocator>::crend

```cpp
reverse_iterator rend();  // (until C++11)
reverse_iterator rend() noexcept;  // (since C++11) (until C++20)
constexpr reverse_iterator rend() noexcept;  // (since C++20)
const_reverse_iterator rend() const;  // (until C++11)
const_reverse_iterator rend() const noexcept;  // (since C++11) (until C++20)
constexpr const_reverse_iterator rend() const noexcept;  // (since C++20)
const_reverse_iterator crend() const;  // (until C++11)
const_reverse_iterator crend() const noexcept;  // (since C++11) (until C++20)
constexpr const_reverse_iterator crend() const noexcept;  // (since C++20)
```

Returns a reverse iterator to the character following the last character of the
reversed string. It corresponds to the character preceding the first character
of the non-reversed string. This character acts as a placeholder, attempting to
access it results in undefined behavior.

### Parameters

(none)

### Return value

Reverse iterator to the character following the last character.

### Complexity

Constant.

### Notes

In libstdc++, `crend()` is not available in C++98 mode.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <iterator>
#include <string>

int main()
{
    std::string p("[A man, a plan, a canal: Panama]");
    std::string q;

    std::copy(p.crbegin(), p.crend(), std::back_inserter(q));
    std::cout << "q = " << q << '\n';

    std::copy(q.crbegin(), q.crend(), p.rbegin());
    std::cout << "p = " << p << '\n';
}
```

Output:

```text
q = ]amanaP :lanac a ,nalp a ,nam A[
p = ]amanaP :lanac a ,nalp a ,nam A[
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 1192 | C++98 | `std::basic_string` did not have the member function
      `crend()` | added

### See also

- **rbegincrbegin (DR*)** — returns a reverse iterator to the beginning (public
  member function)
- **rendcrend** — returns a reverse iterator to the end (public member function
  of `std::basic_string_view<CharT,Traits>`)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/rend*
