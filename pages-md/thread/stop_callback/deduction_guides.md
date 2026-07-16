# deduction guides for `std::stop_callback`

```cpp
template< class Callback >
stop_callback( std::stop_token, Callback ) -> stop_callback<Callback>;  // (since C++20)
```

One deduction guide is provided for `std::stop_callback` to permit deduction
from argument of invocable types.

### Example

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_callback/deduction_guides*
