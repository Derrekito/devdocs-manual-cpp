# C++ named requirements: RangeAdaptorObject (since C++20)

*Range adaptor objects* are customization point objects that accept
`viewable_range` as their first arguments and return a `view`. Some range
adaptor objects are unary, i.e. they take one `viewable_range` as their only
argument. Other range adaptor objects take a `viewable_range` and other trailing
arguments.

If a range adaptor object takes only one argument, it is also a
RangeAdaptorClosureObject.

If a range adaptor object takes more than one argument, it also supports partial
application: let

- `a` be such a range adaptor object, and
- `args...` be arguments (generally suitable for trailing arguments),

expression `a(args...)` has following properties:

- it is valid if and only if for every argument `e` in `args...` such that `E`
  is `decltype((e))`, `std::is_constructible_v<std::decay_t<E>, E>` is `true`,
- when the call is valid, its result object stores a subobject of type
  `std::decay_t<E>` direct-non-list-initialized with `std::forward<E>(e)`, for
  every argument `e` in `args...` (in other words, range adaptor objects bind
  arguments by value),
- the result object is a RangeAdaptorClosureObject,
- calling the RangeAdaptorClosureObject forwards the bound arguments (if any) to
  the associated range adaptor object. The bound arguments (if any) are
  considered to have the value category and cv-qualification of the
  RangeAdaptorClosureObject. In other words, `a(args...)(r)` is equivalent to
  `std::bind_back(a, args...)(r)` (but the former also supports the pipe
  syntax).(since C++23)

Like other customization point objects, let

- `a` be an object of the cv-unqualified version of the type of any range
  adaptor objects,
- `args...` be any group of arguments that satisfies the constraints of the
  `operator()` of the type of `a`,

calls to

- `a(args...)`,
- `std::as_const(a)(args...)`,
- `std::move(a)(args...)`, and
- `std::move(std::as_const(a))(args...)`

are all equivalent.

The result object of each of these expressions is either a `view` object or a
RangeAdaptorClosureObject.

### Notes

`operator()` is unsupported for volatile-qualified or const-volatile-qualified
version of range adaptor object types. Arrays and functions are converted to
pointers while binding.

---
*Source: https://en.cppreference.com/w/cpp/named_req/RangeAdaptorObject*
