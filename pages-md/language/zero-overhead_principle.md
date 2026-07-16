# Zero-overhead principle

The *zero-overhead principle* is a C++ design principle that states:

1. You don't pay for what you don't use.
1. What you do use is just as efficient as what you could reasonably write by
   hand.

In general, this means that no feature should be added to C++ that would impose
any overhead, whether in time or space, greater than a programmer would
introduce without using the feature.

The only two features in the language that do not follow the zero-overhead
principle are runtime type identification and exceptions, and are why most
compilers include a switch to turn them off.

### External links

  1. | Foundations of C++ - Bjarne Stroustrup
  2. | C++ exceptions and alternatives - Bjarne Stroustrup
  3. | De-fragmenting C++ - Making Exceptions and RTTI More Affordable and
      Usable - Herb Sutter
  4. | Bjarne Stroustrup: C++ on Artificial Intelligence (AI) Podcast

---
*Source: https://en.cppreference.com/w/cpp/language/Zero-overhead_principle*
