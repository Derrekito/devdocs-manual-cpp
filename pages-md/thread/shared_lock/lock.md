# std::shared_lock<Mutex>::lock

```cpp
void lock();  // (since C++14)
```

Locks the associated mutex in shared mode. Effectively calls
`mutex()->lock_shared()`.

### Parameters

(none)

### Return value

(none)

### Exceptions

- Any exceptions thrown by `mutex()->lock_shared()`.
- If there is no associated mutex, `std::system_error` with an error code of
  `std::errc::operation_not_permitted`.
- If the associated mutex is already locked by this `shared_lock` (that is,
  owns_lock returns `true`), `std::system_error` with an error code of
  `std::errc::resource_deadlock_would_occur`.

### Example

```cpp
#include <iostream>
#include <mutex>
#include <shared_mutex>
#include <string>
#include <thread>

std::string file = "Original content."; // Simulates a file
std::mutex output_mutex; // mutex that protects output operations.
std::shared_mutex file_mutex; // reader/writer mutex

void read_content(int id)
{
    std::string content;
    {
        std::shared_lock lock(file_mutex, std::defer_lock); // Do not lock it first.
        lock.lock(); // Lock it here.
        content = file;
    }
    std::lock_guard lock(output_mutex);
    std::cout << "Contents read by reader #" << id << ": " << content << '\n';
}

void write_content()
{
    {
        std::lock_guard file_lock(file_mutex);
        file = "New content";
    }
    std::lock_guard output_lock(output_mutex);
    std::cout << "New content saved.\n";
}

int main()
{
    std::cout << "Two readers reading from file.\n"
              << "A writer competes with them.\n";
    std::thread reader1{read_content, 1};
    std::thread reader2{read_content, 2};
    std::thread writer{write_content};
    reader1.join();
    reader2.join();
    writer.join();
    std::cout << "The first few operations to file are done.\n";
    reader1 = std::thread{read_content, 3};
    reader1.join();
}
```

Possible output:

```text
Two readers reading from file.
A writer competes with them.
Contents read by reader #1: Original content.
Contents read by reader #2: Original content.
New content saved.
The first few operations to file are done.
Contents read by reader #3: New content
```

### See also

- **try_lock** — tries to lock the associated mutex (public member function)
- **unlock** — unlocks the associated mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_lock/lock*
