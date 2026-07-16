# Pack indexing (since C++26)

Accesses the element of a parameter pack at a specified index.

### Syntax

```text
id-expression ...[ expression ]    (1)
typedef-name ...[ expression ]    (2)
```

1) Pack indexing expression

2) Pack indexing specifier

- **typedef-name** — an identifier or a simple-template-id that names parameter
  pack
- **id-expression** — an id-expression that names parameter pack
- **expression** — a converted constant expression `I` of type std::size_t
  designated as index where `I` is within the range `[`​0​`,`sizeof...(P)`)` for
  some pack `P` in pack indexing

### Explanation

Pack indexing is a *pack expansion* of the unexpanded parameter pack followed by
an ellipsis and index inside the subscript. There are two kinds of pack
indexing: pack indexing expression and pack indexing specifier.

Let `P` be a non-empty parameter pack containing `P0, P1, ..., Pn-1` and `I` be
a valid index, the instantiation of the expansion `P...[I]` yields the pack
element `PI` of `P`.

Indexing a pack with non-constant expression index `I` is not allowed.

```cpp
int runtime_idx();

void bar(auto... args)
{
    auto a = args...[0];
    const int n = 1;
    auto b = args...[n];
    int m = 2;
    auto c = args...[m]; // error: 'm' is not a constant expression
    auto d = args...[runtime_idx()]; // error: 'runtime_idx()' is not a constant expression
}
```

Indexing a pack of template template parameters is not possible.

```cpp
template <template <typename...> typename... Temps>
using A = Temps...[0]<>; // error: 'Temps' is a pack of template template parameters

template <template <typename...> typename... Temps>
using B = Temps<>...[0]; // error: 'Temps<>' doesn't denote pack name
                         // although it is a simple-template-id
```

### Pack indexing expression

```text
id-expression ...[ expression ]
```

Pack indexing expression denotes the *id-expression*, the expression of pack
element `PI`. The id-expression shall be introduced by the declaration of:

- non-type template parameter pack,
- function parameter pack, or
- lambda init-capture pack.

```cpp
template <std::size_t I, typename... Ts>
constexpr auto element_at(Ts... args)
{
    // 'args' introduced in function parameter pack declaration
    return args...[I];
}

static_assert(element_at<0>(3, 5, 9) == 3);
static_assert(element_at<2>(3, 5, 9) == 9);
static_assert(element_at<3>(3, 5, 9) == 4); // error:  out of bounds
static_assert(element_at<0>() == 1); // error: out of bounds, empty pack

// 'Vals' introduced in non-type template parameter pack declaration
template <std::size_t I, std::size_t... Vals>
constexpr std::size_t double_at = Vals...[I] * 2; // OK

template <std::size_t I, typename... Args>
constexpr auto foo(Args... args) {
    return [...members = args](Args...[I] op)
    {
        // 'members' introduced in lambda init-capture pack
        return members...[I] + op;
    };
}

static_assert(foo<0>(4, "Hello", true)(5) == 9);
static_assert(foo<1>(3, std::string("C++"))("26") == "C++26");
```

Indexing pack of complex expressions other than id-expression is not allowed.

```cpp
template <std::size_t I, auto... Vals>
constexpr auto identity_at = (Vals)...[I]; // error
// use 'Vals...[I]' instead

template <std::size_t I, std::size_t... Vals>
constexpr std::size_t triple_at = (Vals * 3)...[I]; // error
// use 'Vals...[I] * 3' instead

template <std::size_t I, typename... Args>
constexpr decltype(auto) get(Args&&... args) noexcept
{
    return std::forward<Args>(args)...[I]; // error
    // use 'std::forward<Args...[I]>(args...[I])' instead
}
```

Applying `decltype` to pack indexing expression is the same as applying
`decltype` to id-expression.

```cpp
void f()
{
    [](auto... args)
    {
        using T0 = decltype(args...[0]);   // 'T0' is 'double'
        using T1 = decltype((args...[0])); // 'T1' is 'double&'
    }(3.14);
}
```

### Pack indexing specifier

```text
typedef-name ...[ expression ]
```

Pack indexing specifier denotes the *computed-type-specifier*, the type of pack
element `PI`. The typedef-name shall be introduced by the declaration of type
template parameter pack.

```cpp
template <typename... Ts>
using last_type_t = Ts...[sizeof...(Ts) - 1];

static_assert(std::is_same_v<last_type_t<>, int>); // error: out of bounds
static_assert(std::is_same_v<last_type_t<int>, int>);
static_assert(std::is_same_v<last_type_t<bool, char>, char>);
static_assert(std::is_same_v<last_type_t<float, int, bool*>, bool*>);
```

Pack indexing specifier can appear as:

- a simple type specifier,
- a base class specifier,
- a nested name specifier, or
- the type of an explicit destructor call.

Pack indexing specifier can be used in function or constructor parameter list to
establish non-deduced contexts in template argument deduction.

```cpp
template <typename...>
struct type_seq {};

template <typename... Ts>
auto f(Ts...[0] arg, type_seq<Ts...>)
{
    return arg;
}

// OK: "Hello" is implicitly converted to 'std::string_view'
std::same_as<std::string_view> auto a = f("Hello", type_seq<std::string_view>{});

// Error: "Ok" is not convertible to 'int'
std::same_as<int> auto b = f("Ok", type_seq<int, const char*>{});
```

### Notes

Before C++26, `Ts...[N]` was a valid syntax for declaring function parameter
pack of unnamed arrays of size `N`. Since C++26, `Ts...[1]` is interpreted as a
pack indexing specifier which would change the behavior below to #2. To preserve
the first behavior, the function parameter pack must be named like function `g`.

```cpp
template <typename... Ts>
void f(Ts... [1]);

template <typename... Ts>
void g(Ts... args[1]);

void foo()
{
    f<char, bool>(nullptr, nullptr);
    // behavior #1 (before C++26)
    //  calls void 'f<char, bool>(char[1], bool[1])'
    // behavior #2 (since C++26):
    //  error: supposedly called 'void f<char, bool>(bool)'
    //  but provided with 2 arguments instead of 1

    g<char, bool>(nullptr, nullptr);
    // calls 'g<char, bool>(char[1], bool[1])'
}
```

  Feature-test macro | Value | Std | Feature
  `__cpp_pack_indexing` | 202311L | (C++26) | Pack indexing

### Example

---
*Source: https://en.cppreference.com/w/cpp/language/pack_indexing*
