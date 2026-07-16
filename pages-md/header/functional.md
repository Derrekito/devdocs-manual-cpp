# Standard library header <functional>

`<functional>` is the function objects library: it's where callables
get wrapped, combined, and compared. Reach for it when you need to
store an arbitrary callable in a variable or container (`function`,
`move_only_function`, `function_ref`), adapt one callable into another
(`bind`, `bind_front`, `mem_fn`), or pass a comparator/reducer as a
value instead of writing a lambda (`less`, `plus`, `equal_to`, ...). It
also hosts `std::hash`'s primary template and its specializations for
the built-in types; other headers add their own `hash` specializations
here on top.

The callable wrappers differ in ownership: `function` requires its
target to be copy-constructible and owns it; `move_only_function`
(C++23) drops the copyable requirement so move-only callables work;
`function_ref` (C++26) is a cheap non-owning reference to someone
else's callable, so it must not outlive what it refers to.

### Namespaces

- **`placeholders`** (C++11) — the `_1`, `_2`, ... placeholders used in
  a `std::bind` expression

### Callable wrappers

- **function** (C++11) — wraps a callable of any copy-constructible
  type with a given call signature
- **move_only_function** (C++23) — like `function`, but also accepts
  move-only callables
- **copyable_function** (C++26) — a `move_only_function` refinement
  that requires a copy-constructible target
- **function_ref** (C++26) — non-owning reference to any callable with
  a given call signature
- **mem_fn** (C++11) — creates a function object from a pointer to
  member
- **reference_wrapper** (C++11) — copyable, assignable wrapper that
  makes a reference behave like a regular object
- **ref** / **cref** (C++11) — creates a `reference_wrapper`, deducing
  its type
- **unwrap_reference** / **unwrap_ref_decay** (C++20) — the type a
  `reference_wrapper` wraps, or `T` unchanged otherwise
- **bad_function_call** (C++11) — thrown by invoking an empty
  `std::function`

### Binding and invocation

- **bind** (C++11) — binds one or more arguments to a callable
- **bind_front** (C++20) / **bind_back** (C++23) — binds arguments to
  the front, or the back, of a callable's argument list
- **invoke** (C++17) / **invoke_r** (C++23) — invokes any callable with
  given arguments, optionally specifying the return type
- **is_bind_expression** (C++11) — is a type a `std::bind` expression?
- **is_placeholder** (C++11) — is a type a standard placeholder (or
  usable as one)?
- **_1, _2, _3, ...** (C++11) — the placeholder objects themselves

### Arithmetic, comparison, and logical function objects

- **plus** / **minus** / **multiplies** / **divides** / **modulus** /
  **negate** — function objects for `+ - * / % unary-`
- **equal_to** / **not_equal_to** / **greater** / **less** /
  **greater_equal** / **less_equal** — function objects for
  `== != > < >= <=`
- **logical_and** / **logical_or** / **logical_not** — function objects
  for `&& || !`
- **bit_and** / **bit_or** / **bit_xor** — function objects for
  `& | ^`
- **bit_not** (C++14) — function object for unary `~`
- **compare_three_way** (C++20) — function object for `<=>`
- **ranges::equal_to** / **ranges::not_equal_to** / **ranges::greater**
  / **ranges::less** / **ranges::greater_equal** / **ranges::less_equal**
  (C++20) — concept-constrained counterparts of the comparison function
  objects above
- **identity** (C++20) — function object that returns its argument
  unchanged
- **not_fn** (C++17) — wraps a callable so calling it negates the
  result

### Searchers

- **default_searcher** (C++17) — the standard library's own search
  algorithm, wrapped as a callable
- **boyer_moore_searcher** / **boyer_moore_horspool_searcher** (C++17)
  — Boyer-Moore and Boyer-Moore-Horspool substring search

### Hashing

- **hash** (C++11) — the hash function object primary template
- **std::hash** specializations for arithmetic types, enumerations,
  `nullptr_t`, and pointers (C++11)

### Deprecated in C++11, removed in C++17

- **unary_function** / **binary_function** — adaptor-compatible base
  classes
- **binder1st** / **binder2nd**, **bind1st** / **bind2nd** — bind one
  argument to a binary function
- **pointer_to_unary_function** / **pointer_to_binary_function**,
  **ptr_fun** — adaptor-compatible wrappers for function pointers
- **mem_fun_t** family, **mem_fun** — wrappers for a pointer to member
  function, called through an object pointer
- **mem_fun_ref_t** family, **mem_fun_ref** — same, called through an
  object reference

### Deprecated in C++17, removed in C++20

- **unary_negate** / **binary_negate** — wraps a predicate, returning
  its complement
- **not1** / **not2** — constructs a `unary_negate` / `binary_negate`

### See also

- **`<string>`** — specializes `std::hash` for `std::string` and the
  other `basic_string` instantiations
- **`<string_view>`** — specializes `std::hash` for `std::string_view`
  and its instantiations
- **`<system_error>`** — specializes `std::hash` for `std::error_code`
- **`<bitset>`** — specializes `std::hash` for `std::bitset`
- **`<memory>`** — specializes `std::hash` for `std::unique_ptr`,
  `std::shared_ptr`
- **`<typeindex>`** — specializes `std::hash` for `std::type_index`
- **`<vector>`** — specializes `std::hash` for `std::vector<bool>`
- **`<thread>`** — specializes `std::hash` for `std::thread::id`
- **`<optional>`** — specializes `std::hash` for `std::optional`
- **`<variant>`** — specializes `std::hash` for `std::variant`
- **`<coroutine>`** — specializes `std::hash` for
  `std::coroutine_handle`
- **`<stacktrace>`** — specializes `std::hash` for
  `std::stacktrace_entry` and `std::basic_stacktrace`

---
*Source: https://en.cppreference.com/w/cpp/header/functional*
