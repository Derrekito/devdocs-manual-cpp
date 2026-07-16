# std::defer_lock, std::try_to_lock, std::adopt_lock

```cpp
constexpr std::defer_lock_t defer_lock {};  // (since C++11) (until C++17)
inline constexpr std::defer_lock_t defer_lock {};  // (since C++17)
constexpr std::try_to_lock_t try_to_lock {};  // (since C++11) (until C++17)
inline constexpr std::try_to_lock_t try_to_lock {};  // (since C++17)
constexpr std::adopt_lock_t adopt_lock {};  // (since C++11) (until C++17)
inline constexpr std::adopt_lock_t adopt_lock {};  // (since C++17)
```

`std::defer_lock`, `std::try_to_lock` and `std::adopt_lock` are instances of
empty struct tag types `std::defer_lock_t`, `std::try_to_lock_t` and
`std::adopt_lock_t` respectively.

They are used to specify locking strategies for `std::lock_guard`,
`std::unique_lock` and `std::shared_lock`.

- **`defer_lock_t`** — do not acquire ownership of the mutex
- **`try_to_lock_t`** — try to acquire ownership of the mutex without blocking
- **`adopt_lock_t`** — assume the calling thread already has ownership of the
  mutex

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

struct bank_account
{
    explicit bank_account(int balance) : balance{balance} {}
    int balance;
    std::mutex m;
};

void transfer(bank_account& from, bank_account& to, int amount)
{
    if (&from == &to) // avoid deadlock in case of self transfer
        return;

    // lock both mutexes without deadlock
    std::lock(from.m, to.m);
    // make sure both already-locked mutexes are unlocked at the end of scope
    std::lock_guard lock1{from.m, std::adopt_lock};
    std::lock_guard lock2{to.m, std::adopt_lock};

// equivalent approach:
//  std::unique_lock<std::mutex> lock1{from.m, std::defer_lock};
//  std::unique_lock<std::mutex> lock2{to.m, std::defer_lock};
//  std::lock(lock1, lock2);

    from.balance -= amount;
    to.balance += amount;
}

int main()
{
    bank_account my_account{100};
    bank_account your_account{50};

    std::thread t1{transfer, std::ref(my_account), std::ref(your_account), 10};
    std::thread t2{transfer, std::ref(your_account), std::ref(my_account), 5};

    t1.join();
    t2.join();

    std::cout << "my_account.balance = " << my_account.balance << "\n"
                 "your_account.balance = " << your_account.balance << '\n';
}
```

Output:

```text
my_account.balance = 95
your_account.balance = 55
```

### See also

- **defer_lock_ttry_to_lock_tadopt_lock_t (C++11)(C++11)(C++11)** — tag type
  used to specify locking strategy (class)
- **(constructor)** — constructs a `lock_guard`, optionally locking the given
  mutex (public member function of `std::lock_guard<Mutex>`)
- **(constructor)** — constructs a `unique_lock`, optionally locking (i.e.,
  taking ownership of) the supplied mutex (public member function of
  `std::unique_lock<Mutex>`)

---
*Source: https://en.cppreference.com/w/cpp/thread/lock_tag*
