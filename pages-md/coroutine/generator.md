# std::generator

```cpp
template<
    class Ref,
    class V = void,
    class Allocator = void >
class generator
    : public ranges::view_interface<generator<Ref, V, Allocator>>  // (1) (since C++23)
namespace pmr {
    template< class Ref, class V = void >
    using generator =
        std::generator<Ref, V, std::pmr::polymorphic_allocator<>>;
}  // (2) (since C++23)
```

1) The class template `std::generator` presents a `view` of the elements yielded
   by the evaluation of a coroutine.

2) Convenience alias template for the `generator` using the polymorphic
   allocator.

A `std::generator` generates a sequence of elements by repeatedly resuming the
coroutine from which it was returned. Each time a `co_yield` statement is
evaluated, the coroutine produces one element of the sequence. When the
`co_yield` statement is of the form `co_yield ranges::elements_of(rng)`, each
element of the `range` `rng` is successively produced as an element of the
sequence.

`std::generator` models `view` and `input_range`.

The behavior of a program that adds a specialization for `std::generator` is
undefined.

### Data members

- **`active_` (private)** — Internally, each active instance of `std::generator`
  is associated with a stack (handled as if by object of type
  `std::unique_ptr<std::stack<std::coroutine_handle<>>>`). When `begin` is
  called, a new stack is created and the generator is added to the stack. When
  `co_yield ranges::elements_of(rng)` is evaluated in a generator body, `rng` is
  converted to a generator and added to the stack that contains the enclosing
  generator. When a generator iterator is incremented, the coroutine at the top
  of the associated stack is resumed. When a generator finishes (i.e. when
  `final_suspend` is called), it is removed from the stack. (exposition-only
  member object*)
- **`coroutine_` (private)** — `std::coroutine_handle<promise_type>`
  (exposition-only member object*)

### Template parameters

- **Ref** — the reference type (`ranges::range_reference_t`) of the generator.
  If `V` is `void`, both the reference type and the value type are inferred from
  `Ref`
- **V** — the value type (`range_value_t`) of the generator, or `void`
- **Allocator** — an allocator type or `void`

If `Allocator` is not `void`, then:

- `std::allocator_traits<Allocator>::pointer` is a pointer type;
- `Allocator` meets the Allocator requirements.

### Member types

- **`value` (private)** — `std::conditional_t<std::is_void_v<V>,
  std::remove_cvref_t<Ref>, V>;`. `value` is a cv-unqualified object type.
  (exposition-only member type*)
- **`reference` (private)** — `std::conditional_t<std::is_void_v<V>, Ref&&,
  Ref>;`. `reference` is either a reference type, or a cv-unqualified object
  type that models `copy_constructible`. (exposition-only member type*)
- **`yielded`** — `std::conditional_t<std::is_reference_v<reference>, reference,
  const reference&>`.

Let `/*RRef*/` denote:

- `std::remove_reference_t</*reference*/>&&`, if `/*reference*/` is a reference
  type, and
- `/*reference*/` otherwise.

The following concepts are modeled:

- `std::common_reference_with</*reference*/&&, /*value*/&>`,
- `std::common_reference_with</*reference*/&&, /*RRef*/&&>`, and
- `std::common_reference_with</*RRef*/&&, const /*value*/&>`.

### Member functions

- **(constructor)** — constructs a `generator` object (public member function)
- **(destructor)** — effectively destroys the entire stack of yielded
  `generator`s (public member function)
- **operator=** — assigns a `generator` object (public member function)
- **begin** — resumes the initially suspended coroutine and returns an iterator
  to its handle (public member function)
- **end** — returns `std::default_sentinel` (public member function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)

### Nested classes

- **promise_type** — the promise type (public member class)
- ***iterator*** — the iterator type (exposition-only member class*)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_generator` | 202207L | (C++23) | `std::generator` – synchronous
      coroutine generator for ranges

### Example

```cpp
#include <generator>
#include <iostream>
#include <ranges>

std::generator<char> letters(char first)
{
    for (;; co_yield first++);
}

int main()
{
    for (const char ch : letters('a') | std::views::take(26))
        std::cout << ch << ' ';
    std::cout << '\n';
}
```

Output:

```text
a b c d e f g h i j k l m n o p q r s t u v w x y z
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **noop_coroutine (C++20)** — creates a coroutine handle that has no observable
  effects when resumed or destroyed (function)

---
*Source: https://en.cppreference.com/w/cpp/coroutine/generator*
