# operator<<(std::thread::id)

```cpp
template< class CharT, class Traits >
std::basic_ostream<CharT,Traits>&
    operator<<( std::basic_ostream<CharT,Traits>& ost, std::thread::id id );  // (since C++11)
```

Writes a textual representation of a thread identifier `id` to the output stream
`ost`.

If two thread identifiers compare equal, they have identical textual
representations; if they do not compare equal, their representations are
distinct.

### Parameters

- **ost** — output stream to insert the data into
- **id** — thread identifier

### Return value

`ost`

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>
using namespace std::chrono;

int main()
{
    std::thread t1([]{ std::this_thread::sleep_for(256ms); });
    std::thread t2([]{ std::this_thread::sleep_for(512ms); });

    std::clog << t1.get_id() << '\n' << t2.get_id() << '\n';

    t1.join();
    t2.join();
}
```

Possible output:

```text
141592653589793
141421356237309
```

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/id/operator_ltlt*
