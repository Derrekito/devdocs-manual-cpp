# std::execution::sequenced_policy, std::execution::parallel_policy, std::execution::parallel_unsequenced_policy, std::execution::unsequenced_policy

```cpp
class sequenced_policy { /* unspecified */ };  // (1) (since C++17)
class parallel_policy { /* unspecified */ };  // (2) (since C++17)
class parallel_unsequenced_policy { /* unspecified */ };  // (3) (since C++17)
class unsequenced_policy { /* unspecified */ };  // (4) (since C++20)
```

1) The execution policy type used as a unique type to disambiguate parallel
   algorithm overloading and require that a parallel algorithm's execution may
   not be parallelized. The invocations of element access functions in parallel
   algorithms invoked with this policy (usually specified as
   `std::execution::seq`) are indeterminately sequenced in the calling thread.

2) The execution policy type used as a unique type to disambiguate parallel
   algorithm overloading and indicate that a parallel algorithm's execution may
   be parallelized. The invocations of element access functions in parallel
   algorithms invoked with this policy (usually specified as
   `std::execution::par`) are permitted to execute in either the invoking thread
   or in a thread implicitly created by the library to support parallel
   algorithm execution. Any such invocations executing in the same thread are
   indeterminately sequenced with respect to each other.

3) The execution policy type used as a unique type to disambiguate parallel
   algorithm overloading and indicate that a parallel algorithm's execution may
   be parallelized, vectorized, or migrated across threads (such as by a
   parent-stealing scheduler). The invocations of element access functions in
   parallel algorithms invoked with this policy are permitted to execute in an
   unordered fashion in unspecified threads, and unsequenced with respect to one
   another within each thread.

4) The execution policy type used as a unique type to disambiguate parallel
   algorithm overloading and indicate that a parallel algorithm's execution may
   be vectorized, e.g., executed on a single thread using instructions that
   operate on multiple data items.

During the execution of a parallel algorithm with any of these execution
policies, if the invocation of an element access function exits via an uncaught
exception, `std::terminate` is called, but the implementations may define
additional execution policies that handle exceptions differently.

### Notes

When using parallel execution policy, it is the programmer's responsibility to
avoid data races and deadlocks:

```cpp
int a[] = {0, 1};
std::vector<int> v;
std::for_each(std::execution::par, std::begin(a), std::end(a), [&](int i)
{
    v.push_back(i * 2 + 1); // Error: data race
});
```

```cpp
std::atomic<int> x {0};
int a[] = {1, 2};
std::for_each(std::execution::par, std::begin(a), std::end(a), [&](int)
{
    x.fetch_add(1, std::memory_order_relaxed);
    while (x.load(std::memory_order_relaxed) == 1) { } // Error: assumes execution order
});
```

```cpp
int x = 0;
std::mutex m;
int a[] = {1, 2};
std::for_each(std::execution::par, std::begin(a), std::end(a), [&](int)
{
    std::lock_guard<std::mutex> guard(m);
    ++x; // correct
});
```

Unsequenced execution policies are the only case where function calls are
*unsequenced* with respect to each other, meaning they can be interleaved. In
all other situations in C++, they are indeterminately-sequenced (cannot
interleave). Because of that, users are not allowed to allocate or deallocate
memory, acquire mutexes, use non-lockfree `std::atomic` specializations, or, in
general, perform any *vectorization-unsafe* operations when using these policies
(vectorization-unsafe functions are the ones that synchronize-with another
function, e.g. `std::mutex::unlock` synchronizes-with the next
`std::mutex::lock`).

```cpp
int x = 0;
std::mutex m;
int a[] = {1, 2};
std::for_each(std::execution::par_unseq, std::begin(a), std::end(a), [&](int)
{
    std::lock_guard<std::mutex> guard(m); // Error: lock_guard constructor calls m.lock()
    ++x;
});
```

If the implementation cannot parallelize or vectorize (e.g. due to lack of
resources), all standard execution policies can fall back to sequential
execution.

### See also

- **seqparpar_unsequnseq (C++17)(C++17)(C++17)(C++20)** — global execution
  policy objects (constant)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/execution_policy_tag_t*
