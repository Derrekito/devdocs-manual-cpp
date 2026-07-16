# std::predicate

```cpp
template< class F, class... Args >
concept predicate =
    std::regular_invocable<F, Args...> &&
    boolean-testable<std::invoke_result_t<F, Args...>>;  // (since C++20)
```

The concept std::predicate<F, Args...> specifies that `F` is a predicate that
accepts arguments whose types and value categories are encoded by `Args...`,
i.e., it can be invoked with these arguments to produce a `boolean-testable`
result.

Note that `regular_invocable` requires the invocation to not modify either the
callable object or the arguments and be equality-preserving.

### Equality preservation

Expressions declared in requires expressions of the standard library concepts
are required to be equality-preserving (except where stated otherwise).

---
*Source: https://en.cppreference.com/w/cpp/concepts/predicate*
