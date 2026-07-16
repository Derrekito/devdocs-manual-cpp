# std::async

```cpp
template< class Function, class... Args >
std::future<typename std::result_of<typename std::decay<Function>::type(
        typename std::decay<Args>::type...)>::type>
    async( Function&& f, Args&&... args );  // (since C++11) (until C++17)
template< class Function, class... Args >
std::future<std::invoke_result_t<std::decay_t<Function>,
                                 std::decay_t<Args>...>>
    async( Function&& f, Args&&... args );  // (since C++17) (until C++20)
template< class Function, class... Args >
[[nodiscard]] std::future<std::invoke_result_t<std::decay_t<Function>,
                                               std::decay_t<Args>...>>
    async( Function&& f, Args&&... args );  // (since C++20)
template< class Function, class... Args >
std::future<typename std::result_of<typename std::decay<Function>::type(
        typename std::decay<Args>::type...)>::type>
    async( std::launch policy, Function&& f, Args&&... args );  // (since C++11) (until C++17)
template< class Function, class... Args >
std::future<std::invoke_result_t<std::decay_t<Function>,
                                 std::decay_t<Args>...>>
    async( std::launch policy, Function&& f, Args&&... args );  // (since C++17) (until C++20)
template< class Function, class... Args >
[[nodiscard]] std::future<std::invoke_result_t<std::decay_t<Function>,
                                               std::decay_t<Args>...>>
    async( std::launch policy, Function&& f, Args&&... args );  // (since C++20)
```

The function template `std::async` runs the function `f` asynchronously
(potentially in a separate thread which might be a part of a thread pool) and
returns a `std::future` that will eventually hold the result of that function
call.

1) Behaves as if (2) is called with `policy` being `std::launch::async |
   std::launch::deferred`.

2) Calls a function `f` with arguments `args` according to a specific launch
   policy `policy` (see below).

The call to `std::async` *synchronizes-with* (as defined in `std::memory_order`)
the call to `f`, and the completion of `f` is *sequenced-before* making the
shared state ready.

If `INVOKE(decay-copy(std::forward<F>(f)),
decay-copy(std::forward<Args>(args))...)` is not a valid expression, the program
is ill-formed.
*(until C++20)*

The program is ill-formed if
- `std::is_constructible_v<std::decay_t<F>, F>` is `false`,
- `std::is_constructible_v<std::decay_t<Arg_i>, Arg_i>` is `false` for any type
  `Arg_i` in `Args`, or
- `std::is_invocable_v<std::decay_t<F>, std::decay_t<Args>...>` is `false`.
*(since C++20)*

### Parameters

- **f** — Callable object to call
- **args...** — parameters to pass to `f`
- **policy** — bitmask value, where individual bits control the allowed methods
  of execution Bit Explanation `std::launch::async` enable asynchronous
  evaluation `std::launch::deferred` enable lazy evaluation
- **`std::launch::async`** — enable asynchronous evaluation
- **`std::launch::deferred`** — enable lazy evaluation

### Return value

`std::future` referring to the shared state created by this call to
`std::async`.

### Launch policies

#### Async invocation

If the *async* flag is set (i.e. `(policy & std::launch::async) != 0`), then

`std::async` calls `INVOKE(decay-copy(std::forward<F>(f)),
decay-copy(std::forward<Args>(args))...)` as if in a new thread of execution
represented by a `std::thread` object.
*(until C++23)*

`std::async` calls `std::invoke(auto(std::forward<F>(f)),
auto(std::forward<Args>(args))...)` as if in a new thread of execution
represented by a `std::thread` object.
*(since C++23)*

The calls of `decay-copy` are evaluated(until C++23)The values produced by
`auto` are materialized(since C++23) in the current thread. If the function `f`
returns a value or throws an exception, it is stored in the shared state
accessible through the `std::future` that `std::async` returns to the caller.

#### Deferred invocation

If the *deferred* flag is set (i.e. `(policy & std::launch::deferred) != 0`),
then `std::async` stores `decay-copy(std::forward<F>(f))` and
`decay-copy(std::forward<Args>(args))...`(until C++23)`auto(std::forward<F>(f))`
and `auto(std::forward<Args>(args))...`(since C++23) in the shared state.

*Lazy evaluation* is performed:

- The first call to a non-timed wait function on the `std::future` that
  `std::async` returned to the caller will evaluate `INVOKE(std::move(g),
  std::move(xyz))` in the current thread (which does not have to be the thread
  that originally called `std::async`), where
- The result or exception is placed in the shared state associated with the
  returned `std::future` and only then it is made ready. All further accesses to
  the same `std::future` will return the result immediately.

#### Other policies

If neither `std::launch::async` nor `std::launch::deferred`, nor any
implementation-defined policy flag is set in `policy`, the behavior is
undefined.

### Policy selection

If more than one flag is set, it is implementation-defined which policy is
selected. For the default (both the `std::launch::async` and
`std::launch::deferred` flags are set in `policy`), standard recommends (but
does not require) utilizing available concurrency, and deferring any additional
tasks.

If the `std::launch::async` policy is chosen,

- a call to a waiting function on an asynchronous return object that shares the
  shared state created by this `std::async` call blocks until the associated
  thread has completed, as if joined, or else time out; and
- the associated thread completion *synchronizes-with* the successful return
  from the first function that is waiting on the shared state, or with the
  return of the last function that releases the shared state, whichever comes
  first.

### Exceptions

Throws

- `std::bad_alloc`, if the memory for the internal data structures cannot be
  allocated, or
- `std::system_error` with error condition
  `std::errc::resource_unavailable_try_again`, if `policy == std::launch::async`
  and the implementation is unable to start a new thread. If `policy` is
  `std::launch::async | std::launch::deferred` or has additional bits set, it
  will fall back to deferred invocation or the implementation-defined policies
  in this case.

### Notes

The implementation may extend the behavior of the first overload of `std::async`
by enabling additional (implementation-defined) bits in the default launch
policy.

Examples of implementation-defined launch policies are the sync policy (execute
immediately, within the `std::async` call) and the task policy (similar to
`std::async`, but thread-locals are not cleared)

If the `std::future` obtained from `std::async` is not moved from or bound to a
reference, the destructor of the `std::future` will block at the end of the full
expression until the asynchronous operation completes, essentially making code
such as the following synchronous:

```cpp
std::async(std::launch::async, []{ f(); }); // temporary's dtor waits for f()
std::async(std::launch::async, []{ g(); }); // does not start until f() completes
```

Note that the destructors of `std::future`s obtained by means other than a call
to `std::async` never block.

### Example

```cpp
#include <algorithm>
#include <future>
#include <iostream>
#include <mutex>
#include <numeric>
#include <string>
#include <vector>

std::mutex m;

struct X
{
    void foo(int i, const std::string& str)
    {
        std::lock_guard<std::mutex> lk(m);
        std::cout << str << ' ' << i << '\n';
    }

    void bar(const std::string& str)
    {
        std::lock_guard<std::mutex> lk(m);
        std::cout << str << '\n';
    }

    int operator()(int i)
    {
        std::lock_guard<std::mutex> lk(m);
        std::cout << i << '\n';
        return i + 10;
    }
};

template<typename RandomIt>
int parallel_sum(RandomIt beg, RandomIt end)
{
    auto len = end - beg;
    if (len < 1000)
        return std::accumulate(beg, end, 0);

    RandomIt mid = beg + len / 2;
    auto handle = std::async(std::launch::async,
                             parallel_sum<RandomIt>, mid, end);
    int sum = parallel_sum(beg, mid);
    return sum + handle.get();
}

int main()
{
    std::vector<int> v(10000, 1);
    std::cout << "The sum is " << parallel_sum(v.begin(), v.end()) << '\n';

    X x;
    // Calls (&x)->foo(42, "Hello") with default policy:
    // may print "Hello 42" concurrently or defer execution
    auto a1 = std::async(&X::foo, &x, 42, "Hello");
    // Calls x.bar("world!") with deferred policy
    // prints "world!" when a2.get() or a2.wait() is called
    auto a2 = std::async(std::launch::deferred, &X::bar, x, "world!");
    // Calls X()(43); with async policy
    // prints "43" concurrently
    auto a3 = std::async(std::launch::async, X(), 43);
    a2.wait();                     // prints "world!"
    std::cout << a3.get() << '\n'; // prints "53"
} // if a1 is not done at this point, destructor of a1 prints "Hello 42" here
```

Possible output:

```text
The sum is 10000
43
world!
53
Hello 42
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2021 | C++11 | return type incorrect and value category of arguments
      unclear in the deferred case | corrected return type and clarified that
      rvalues are used
  LWG 2078 | C++11 | it was unclear whether `std::system_error` may be thrown if
      `policy` specifies other launch policies besides `std::launch::async` |
      can only be thrown if `policy == std::launch::async`
  LWG 2100 | C++11 | timed waiting functions could not timeout if
      `std::launch::async` policy is used | allowed
  LWG 2120 | C++11 | the behavior was unclear if no standard or
      implementation-defined policy is set | the behavior is undefined in this
      case
  LWG 2752 | C++11 | `std::async` might not throw `std::bad_alloc` if the memory
      for the internal data structures cannot be allocated | throws
  LWG 3476 | C++11 | `Function` and `Args...` were required to be
      MoveConstructible while no additional move constructions specified |
      requirements removed

### See also

- **future (C++11)** — waits for a value that is set asynchronously (class
  template)

---
*Source: https://en.cppreference.com/w/cpp/thread/async*
