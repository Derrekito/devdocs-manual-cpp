# std::multimap<Key,T,Compare,Allocator>::rend, std::multimap<Key,T,Compare,Allocator>::crend

```cpp
reverse_iterator rend();  // (until C++11)
reverse_iterator rend() noexcept;  // (since C++11)
const_reverse_iterator rend() const;  // (until C++11)
const_reverse_iterator rend() const noexcept;  // (since C++11)
const_reverse_iterator crend() const noexcept;  // (3) (since C++11)
```

Returns a reverse iterator to the element following the last element of the
reversed `multimap`. It corresponds to the element preceding the first element
of the non-reversed `multimap`. This element acts as a placeholder, attempting
to access it results in undefined behavior.

### Parameters

(none)

### Return value

Reverse iterator to the element following the last element.

### Complexity

Constant.

### Example

```cpp
#include <chrono>
#include <iomanip>
#include <iostream>
#include <map>
#include <string_view>
using namespace std::chrono;

// until C++20 chrono operator<< ready
std::ostream& operator<<(std::ostream& os, const year_month_day& ymd)
{
    return os << std::setfill('0') << static_cast<int>(ymd.year()) << '/'
              << std::setw(2) << static_cast<unsigned>(ymd.month()) << '/'
              << std::setw(2) << static_cast<unsigned>(ymd.day());
}

int main()
{
    const std::multimap<year_month_day, int> messages
    {
        {February/17/2023, 10},
        {February/17/2023, 20},
        {February/16/2022, 30},
        {October/22/2022, 40},
        {June/14/2022, 50},
        {November/23/2021, 60},
        {December/10/2022, 55},
        {December/12/2021, 45},
        {April/1/2020, 42},
        {April/1/2020, 24}
    };

    std::cout << "Messages received, reverse date order:\n";
    for (auto it = messages.crbegin(); it != messages.crend(); ++it)
        std::cout << it->first << " : " << it->second << '\n';
}
```

Possible output:

```text
Messages received, reverse date order:
2023/02/17 : 20
2023/02/17 : 10
2022/12/10 : 55
2022/10/22 : 40
2022/06/14 : 50
2022/02/16 : 30
2021/12/12 : 45
2021/11/23 : 60
2020/04/01 : 24
2020/04/01 : 42
```

### See also

- **rbegincrbegin (C++11)** — returns a reverse iterator to the beginning
  (public member function)
- **rendcrend (C++14)** — returns a reverse end iterator for a container or
  array (function template)

---
*Source: https://en.cppreference.com/w/cpp/container/multimap/rend*
