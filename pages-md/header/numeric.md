# Standard library header <numeric>

This header is part of the numeric library.

**Functions**

- **iota (C++11)** — fills a range with successive increments of the starting
  value (function template)
- **ranges::iota (C++23)** — fills a range with successive increments of the
  starting value (niebloid)
- **accumulate** — sums up or folds a range of elements (function template)
- **reduce (C++17)** — similar to `std::accumulate`, except out of order
  (function template)
- **transform_reduce (C++17)** — applies an invocable, then reduces out of order
  (function template)
- **inner_product** — computes the inner product of two ranges of elements
  (function template)
- **adjacent_difference** — computes the differences between adjacent elements
  in a range (function template)
- **partial_sum** — computes the partial sum of a range of elements (function
  template)
- **inclusive_scan (C++17)** — similar to `std::partial_sum`, includes the ith
  input element in the ith sum (function template)
- **exclusive_scan (C++17)** — similar to `std::partial_sum`, excludes the ith
  input element from the ith sum (function template)
- **transform_inclusive_scan (C++17)** — applies an invocable, then calculates
  inclusive scan (function template)
- **transform_exclusive_scan (C++17)** — applies an invocable, then calculates
  exclusive scan (function template)
- **gcd (C++17)** — computes the greatest common divisor of two integers
  (function template)
- **lcm (C++17)** — computes the least common multiple of two integers (function
  template)
- **midpoint (C++20)** — midpoint between two numbers or pointers (function
  template)
- **add_sat (C++26)** — saturating addition operation on two integers (function
  template)
- **sub_sat (C++26)** — saturating subtraction operation on two integers
  (function template)
- **mul_sat (C++26)** — saturating multiplication operation on two integers
  (function template)
- **div_sat (C++26)** — saturating division operation on two integers (function
  template)
- **saturate_cast (C++26)** — returns an integer value clamped to the range of a
  another integer type (function template)

### Synopsis

```cpp
namespace std {
  // accumulate
  template<class InputIt, class T>
    constexpr T accumulate(InputIt first, InputIt last, T init);
  template<class InputIt, class T, class BinaryOperation>
    constexpr T accumulate(InputIt first, InputIt last, T init, BinaryOperation binary_op);

  // reduce
  template<class InputIt>
    constexpr typename iterator_traits<InputIt>::value_type
      reduce(InputIt first, InputIt last);
  template<class InputIt, class T>
    constexpr T reduce(InputIt first, InputIt last, T init);
  template<class InputIt, class T, class BinaryOperation>
    constexpr T reduce(InputIt first, InputIt last, T init, BinaryOperation binary_op);
  template<class ExecutionPolicy, class ForwardIt>
    typename iterator_traits<ForwardIt>::value_type
      reduce(ExecutionPolicy&& exec,
             ForwardIt first, ForwardIt last);
  template<class ExecutionPolicy, class ForwardIt, class T>
    T reduce(ExecutionPolicy&& exec,
             ForwardIt first, ForwardIt last, T init);
  template<class ExecutionPolicy, class ForwardIt, class T, class BinaryOperation>
    T reduce(ExecutionPolicy&& exec,
             ForwardIt first, ForwardIt last, T init, BinaryOperation binary_op);

  // inner product
  template<class InputIt1, class InputIt2, class T>
    constexpr T inner_product(InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, T init);
  template<class InputIt1, class InputIt2, class T,
           class BinaryOperation1, class BinaryOperation2>
    constexpr T inner_product(InputIt1 first1, InputIt1 last1,
                              InputIt2 first2, T init,
                              BinaryOperation1 binary_op1,
                              BinaryOperation2 binary_op2);

  // transform reduce
  template<class InputIt1, class InputIt2, class T>
    constexpr T transform_reduce(InputIt1 first1, InputIt1 last1,
                                 InputIt2 first2,
                                 T init);
  template<class InputIt1, class InputIt2, class T,
           class BinaryOperation1, class BinaryOperation2>
    constexpr T transform_reduce(InputIt1 first1, InputIt1 last1,
                                 InputIt2 first2,
                                 T init,
                                 BinaryOperation1 binary_op1,
                                 BinaryOperation2 binary_op2);
  template<class InputIt, class T,
           class BinaryOperation, class UnaryOperation>
    constexpr T transform_reduce(InputIt first, InputIt last,
                                 T init,
                                 BinaryOperation binary_op, UnaryOperation unary_op);
  template<class ExecutionPolicy,
           class ForwardIt1, class ForwardIt2, class T>
    T transform_reduce(ExecutionPolicy&& exec,
                       ForwardIt1 first1, ForwardIt1 last1,
                       ForwardIt2 first2,
                       T init);
  template<class ExecutionPolicy,
           class ForwardIt1, class ForwardIt2, class T,
           class BinaryOperation1, class BinaryOperation2>
    T transform_reduce(ExecutionPolicy&& exec,
                       ForwardIt1 first1, ForwardIt1 last1,
                       ForwardIt2 first2,
                       T init,
                       BinaryOperation1 binary_op1,
                       BinaryOperation2 binary_op2);
  template<class ExecutionPolicy,
           class ForwardIt, class T,
           class BinaryOperation, class UnaryOperation>
    T transform_reduce(ExecutionPolicy&& exec,
                       ForwardIt first, ForwardIt last,
                       T init,
                       BinaryOperation binary_op, UnaryOperation unary_op);

  // partial sum
  template<class InputIt, class OutputIt>
    constexpr OutputIt partial_sum(InputIt first,
                                   InputIt last,
                                   OutputIt result);
  template<class InputIt, class OutputIt, class BinaryOperation>
    constexpr OutputIt partial_sum(InputIt first,
                                   InputIt last,
                                   OutputIt result,
                                   BinaryOperation binary_op);

  // exclusive scan
  template<class InputIt, class OutputIt, class T>
    constexpr OutputIt exclusive_scan(InputIt first, InputIt last,
                                      OutputIt result,
                                      T init);
  template<class InputIt, class OutputIt, class T, class BinaryOperation>
    constexpr OutputIt exclusive_scan(InputIt first, InputIt last,
                                      OutputIt result,
                                      T init, BinaryOperation binary_op);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T>
    ForwardIt2 exclusive_scan(ExecutionPolicy&& exec,
                              ForwardIt1 first, ForwardIt1 last,
                              ForwardIt2 result,
                              T init);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2, class T,
           class BinaryOperation>
    ForwardIt2 exclusive_scan(ExecutionPolicy&& exec,
                              ForwardIt1 first, ForwardIt1 last,
                              ForwardIt2 result,
                              T init, BinaryOperation binary_op);

  // inclusive scan
  template<class InputIt, class OutputIt>
    constexpr OutputIt inclusive_scan(InputIt first, InputIt last, OutputIt result);
  template<class InputIt, class OutputIt, class BinaryOperation>
    constexpr OutputIt inclusive_scan(InputIt first, InputIt last,
                                      OutputIt result,
                                      BinaryOperation binary_op);
  template<class InputIt, class OutputIt, class BinaryOperation, class T>
    constexpr OutputIt inclusive_scan(InputIt first, InputIt last,
                                      OutputIt result,
                                      BinaryOperation binary_op, T init);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2>
    ForwardIt2 inclusive_scan(ExecutionPolicy&& exec,
                              ForwardIt1 first, ForwardIt1 last,
                              ForwardIt2 result);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
           class BinaryOperation>
    ForwardIt2 inclusive_scan(ExecutionPolicy&& exec,
                              ForwardIt1 first, ForwardIt1 last,
                              ForwardIt2 result,
                              BinaryOperation binary_op);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
           class BinaryOperation, class T>
    ForwardIt2 inclusive_scan(ExecutionPolicy&& exec,
                              ForwardIt1 first, ForwardIt1 last,
                              ForwardIt2 result,
                              BinaryOperation binary_op, T init);

  // transform exclusive scan
  template<class InputIt, class OutputIt, class T,
           class BinaryOperation, class UnaryOperation>
    constexpr OutputIt transform_exclusive_scan(InputIt first, InputIt last,
                                                OutputIt result,
                                                T init,
                                                BinaryOperation binary_op,
                                                UnaryOperation unary_op);
  template<class ExecutionPolicy,
           class ForwardIt1, class ForwardIt2, class T,
           class BinaryOperation, class UnaryOperation>
    ForwardIt2 transform_exclusive_scan(ExecutionPolicy&& exec,
                                        ForwardIt1 first, ForwardIt1 last,
                                        ForwardIt2 result,
                                        T init,
                                        BinaryOperation binary_op,
                                        UnaryOperation unary_op);

  // transform inclusive scan
  template<class InputIt, class OutputIt,
           class BinaryOperation, class UnaryOperation>
    constexpr OutputIt transform_inclusive_scan(InputIt first, InputIt last,
                                                OutputIt result,
                                                BinaryOperation binary_op,
                                                UnaryOperation unary_op);
  template<class InputIt, class OutputIt,
           class BinaryOperation, class UnaryOperation, class T>
    constexpr OutputIt transform_inclusive_scan(InputIt first, InputIt last,
                                                OutputIt result,
                                                BinaryOperation binary_op,
                                                UnaryOperation unary_op,
                                                T init);
  template<class ExecutionPolicy,
           class ForwardIt1, class ForwardIt2,
           class BinaryOperation, class UnaryOperation>
    ForwardIt2 transform_inclusive_scan(ExecutionPolicy&& exec,
                                        ForwardIt1 first, ForwardIt1 last,
                                        ForwardIt2 result,
                                        BinaryOperation binary_op,
                                        UnaryOperation unary_op);
  template<class ExecutionPolicy,
           class ForwardIt1, class ForwardIt2,
           class BinaryOperation, class UnaryOperation, class T>
    ForwardIt2 transform_inclusive_scan(ExecutionPolicy&& exec,
                                        ForwardIt1 first, ForwardIt1 last,
                                        ForwardIt2 result,
                                        BinaryOperation binary_op,
                                        UnaryOperation unary_op,
                                        T init);

  // adjacent difference
  template<class InputIt, class OutputIt>
    constexpr OutputIt adjacent_difference(InputIt first, InputIt last,
                                           OutputIt result);
  template<class InputIt, class OutputIt, class BinaryOperation>
    constexpr OutputIt adjacent_difference(InputIt first, InputIt last,
                                           OutputIt result,
                                           BinaryOperation binary_op);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2>
    ForwardIt2 adjacent_difference(ExecutionPolicy&& exec,
                                   ForwardIt1 first, ForwardIt1 last,
                                   ForwardIt2 result);
  template<class ExecutionPolicy, class ForwardIt1, class ForwardIt2,
           class BinaryOperation>
    ForwardIt2 adjacent_difference(ExecutionPolicy&& exec,
                                   ForwardIt1 first, ForwardIt1 last,
                                   ForwardIt2 result,
                                   BinaryOperation binary_op);

  // iota
  template<class ForwardIt, class T>
    constexpr void iota(ForwardIt first, ForwardIt last, T value);

  namespace ranges {
    template<class O, class T>
      using iota_result = out_value_result<O, T>;

    template<input_or_output_iterator O, sentinel_for<O> S, weakly_incrementable T>
      requires indirectly_writable<O, const T&>
      constexpr iota_result<O, T> iota(O first, S last, T value);

    template<weakly_incrementable T, output_range<const T&> R>
      constexpr iota_result<borrowed_iterator_t<R>, T> iota(R&& r, T value);
  }

  // greatest common divisor
  template<class M, class N>
    constexpr common_type_t<M, N> gcd(M m, N n);

  // least common multiple
  template<class M, class N>
    constexpr common_type_t<M, N> lcm(M m, N n);

  // midpoint
  template<class T>
    constexpr T midpoint(T a, T b) noexcept;
  template<class T>
    constexpr T* midpoint(T* a, T* b);

  // saturation arithmetic
  template<class T>
    constexpr T add_sat(T x, T y) noexcept;           // freestanding
  template<class T>
    constexpr T sub_sat(T x, T y) noexcept;           // freestanding
  template<class T>
    constexpr T mul_sat(T x, T y) noexcept;           // freestanding
  template<class T>
    constexpr T div_sat(T x, T y) noexcept;           // freestanding
  template<class T, class U>
    constexpr T saturate_cast(U x) noexcept;          // freestanding
}
```

---
*Source: https://en.cppreference.com/w/cpp/header/numeric*
