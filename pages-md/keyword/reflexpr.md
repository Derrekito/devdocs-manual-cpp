# C++ keyword: reflexpr (reflection TS)

Experimental Feature The functionality described on this page is part of the
Reflection Technical Specification ISO/IEC TS 23619 (reflection TS).

### Usage

1) Gets the member list of a class type, or the enumerator list of an enum type.

2) Gets the name of type and member.

3) Detects whether a data member is static or constexpr.

4) Detects whether member function is virtual, public, protected or private.

5) Get the *row* and *column* of the source code when the type defines.

### Example

`reflexpr` provides us the meta info of the object via *meta-object types*. Note
that `std::reflect::get_data_members_t` make programmers able to visit any class
just like `std::tuple`.

```cpp
#include <string>
#include <vector>

struct S
{
    int b;
    std::string s;
    std::vector<std::string> v;
};

// Reflection TS
#include <experimental/reflect>
using meta_S = reflexpr(S);
using mem = std::reflect::get_data_members_t<meta_S>;
using meta = std::reflect::get_data_members_t<mem>;
static_assert(std::reflect::is_public_v<meta>); // successful

int main() {}
```

We can also know the name info from `reflexpr`:

```cpp
#include <iostream>
#include <string>
#include <string_view>
// Reflection TS
#include <experimental/reflect>

template<typename Tp>
constexpr std::string_view nameof()
{
    using TpInfo = reflexpr(Tp);
    using aliased_Info = std::experimental::reflect::get_aliased_t<TpInfo>;
    return std::experimental::reflect::get_name_v<aliased_Info>;
}

int main()
{
    std::cout << nameof<std::string>() << '\n';
    static_assert(nameof<std::string>() == "basic_string"); // successful
}
```

This is an example of getting the *scope* of a type in the Reflection TS.

```cpp
namespace Foo
{
    struct FooFoo
    {
        int FooFooFoo;
    };
}
namespace Bar
{
    using BarBar = ::Foo::FooFoo;
}
using BarBarInfo = reflexpr(::Bar::BarBar);
using BarBarScope = ::std::experimental::reflect::get_scope_t<BarBarInfo>; // Bar, not Foo

struct Spam
{
    int SpamSpam;
};
struct Grok
{
    using GrokGrok = Spam::SpamSpam;
};
using GrokGrokInfo = reflexpr(::Grok::GrokGrok);
using GrokGrokScope = std::experimental::reflect::get_scope_t<GrokGrokInfo>; // Grok, not Spam
```

---
*Source: https://en.cppreference.com/w/cpp/keyword/reflexpr*
